# Layout Algorithms for Obsidian Canvas

Detailed algorithms for positioning nodes in MindMap and Freeform layouts.

## Layout Principles

### Universal Spacing Constants

```
HORIZONTAL_SPACING = 320  // Minimum horizontal space between node centers
VERTICAL_SPACING = 200    // Minimum vertical space between node centers
NODE_PADDING = 20         // Internal padding within nodes
```

### Collision Detection

Before finalizing any node position, verify:

```
def check_collision(node1, node2):
    """Returns True if nodes overlap or are too close"""
    center1_x = node1.x + node1.width / 2
    center1_y = node1.y + node1.height / 2
    center2_x = node2.x + node2.width / 2
    center2_y = node2.y + node2.height / 2

    dx = abs(center1_x - center2_x)
    dy = abs(center1_y - center2_y)

    min_dx = (node1.width + node2.width) / 2 + HORIZONTAL_SPACING
    min_dy = (node1.height + node2.height) / 2 + VERTICAL_SPACING

    return dx < min_dx or dy < min_dy
```

## MindMap Layout Algorithm

### 1. Radial Tree Layout

Place root at center, arrange children radially.

#### Step 1: Position Root Node

```
root = {
    "x": 0 - (root_width / 2),  # Center horizontally
    "y": 0 - (root_height / 2), # Center vertically
    "width": root_width,
    "height": root_height
}
```

#### Step 2: Calculate Primary Branch Positions

Distribute first-level children around root:

```
def position_primary_branches(root, children, radius=400):
    """Position first-level children in a circle around root"""
    n = len(children)
    angle_step = 2 * pi / n

    positions = []
    for i, child in enumerate(children):
        angle = i * angle_step

        # Calculate position on circle
        x = root.center_x + radius * cos(angle) - child.width / 2
        y = root.center_y + radius * sin(angle) - child.height / 2

        positions.append({"x": x, "y": y})

    return positions
```

Radius Selection:
- Small canvases (≤10 children): 400px
- Medium canvases (11-20 children): 500px
- Large canvases (>20 children): 600px

#### Step 3: Position Secondary Branches

For each primary branch, arrange its children:

Horizontal Layout (preferred for most cases):

```
def position_secondary_horizontal(parent, children, distance=350):
    """Arrange children horizontally to the right of parent"""
    n = len(children)
    total_height = sum(child.height for child in children)
    total_spacing = (n - 1) * VERTICAL_SPACING

    # Start position (top of vertical arrangement)
    start_y = parent.center_y - (total_height + total_spacing) / 2

    positions = []
    current_y = start_y

    for child in children:
        x = parent.x + parent.width + distance
        y = current_y

        positions.append({"x": x, "y": y})
        current_y += child.height + VERTICAL_SPACING

    return positions
```

Vertical Layout (for left/right primary branches):

```
def position_secondary_vertical(parent, children, distance=250):
    """Arrange children vertically below parent"""
    n = len(children)
    total_width = sum(child.width for child in children)
    total_spacing = (n - 1) * HORIZONTAL_SPACING

    # Start position (left of horizontal arrangement)
    start_x = parent.center_x - (total_width + total_spacing) / 2

    positions = []
    current_x = start_x

    for child in children:
        x = current_x
        y = parent.y + parent.height + distance

        positions.append({"x": x, "y": y})
        current_x += child.width + HORIZONTAL_SPACING

    return positions
```

#### Step 4: Balance and Adjust

After initial placement, check for collisions and adjust:

```
def balance_layout(nodes):
    """Adjust nodes to prevent overlaps"""
    max_iterations = 10

    for iteration in range(max_iterations):
        collisions = find_all_collisions(nodes)
        if not collisions:
            break

        for node1, node2 in collisions:
            # Move node2 away from node1
            dx = node2.center_x - node1.center_x
            dy = node2.center_y - node1.center_y
            distance = sqrt(dx*dx + dy*dy)

            # Calculate required distance
            min_dist = calculate_min_distance(node1, node2)

            if distance > 0:
                # Move proportionally
                move_x = (dx / distance) * (min_dist - distance) / 2
                move_y = (dy / distance) * (min_dist - distance) / 2

                node2.x += move_x
                node2.y += move_y
```

### 2. Tree Layout (Hierarchical Top-Down)

Alternative for deep hierarchies.

#### Positioning Formula

```
def position_tree_layout(root, tree):
    """Top-down tree layout"""
    # Level 0 (root)
    root.x = 0 - root.width / 2
    root.y = 0 - root.height / 2

    # Process each level
    for level in range(1, max_depth):
        nodes_at_level = get_nodes_at_level(tree, level)

        # Calculate horizontal spacing
        total_width = sum(node.width for node in nodes_at_level)
        total_spacing = (len(nodes_at_level) - 1) * HORIZONTAL_SPACING

        start_x = -(total_width + total_spacing) / 2
        y = level * (150 + VERTICAL_SPACING)  # 150px level height

        current_x = start_x
        for node in nodes_at_level:
            node.x = current_x
            node.y = y
            current_x += node.width + HORIZONTAL_SPACING
```

## Freeform Layout Algorithm

### 1. Content-Based Grouping

First, identify natural groupings in content:

```
def identify_groups(nodes, content_structure):
    """Group nodes by semantic relationships"""
    groups = []

    # Analyze content structure
    for section in content_structure:
        group_nodes = [node for node in nodes if node.section == section]

        if len(group_nodes) > 1:
            groups.append({
                "label": section.title,
                "nodes": group_nodes
            })

    return groups
```

### 2. Grid-Based Zone Layout

Divide canvas into zones for different groups:

```
def layout_zones(groups, canvas_width=2000, canvas_height=1500):
    """Arrange groups in grid zones"""
    n_groups = len(groups)

    # Calculate grid dimensions
    cols = ceil(sqrt(n_groups))
    rows = ceil(n_groups / cols)

    zone_width = canvas_width / cols
    zone_height = canvas_height / rows

    # Assign zones
    zones = []
    for i, group in enumerate(groups):
        col = i % cols
        row = i // cols

        zone = {
            "x": col * zone_width - canvas_width / 2,
            "y": row * zone_height - canvas_height / 2,
            "width": zone_width * 0.9,  # Leave 10% margin
            "height": zone_height * 0.9,
            "group": group
        }
        zones.append(zone)

    return zones
```

### 3. Within-Zone Node Positioning

Position nodes within each zone:

Option A: Organic Flow

```
def position_organic(zone, nodes):
    """Organic, flowing arrangement within zone"""
    positions = []

    # Start at zone top-left with margin
    current_x = zone.x + 50
    current_y = zone.y + 50
    row_height = 0

    for node in nodes:
        # Check if node fits in current row
        if current_x + node.width > zone.x + zone.width - 50:
            # Move to next row
            current_x = zone.x + 50
            current_y += row_height + VERTICAL_SPACING
            row_height = 0

        positions.append({
            "x": current_x,
            "y": current_y
        })

        current_x += node.width + HORIZONTAL_SPACING
        row_height = max(row_height, node.height)

    return positions
```

Option B: Structured Grid

```
def position_grid(zone, nodes):
    """Grid arrangement within zone"""
    n = len(nodes)
    cols = ceil(sqrt(n))
    rows = ceil(n / cols)

    cell_width = (zone.width - 100) / cols  # 50px margin each side
    cell_height = (zone.height - 100) / rows

    positions = []
    for i, node in enumerate(nodes):
        col = i % cols
        row = i // cols

        # Center node in cell
        x = zone.x + 50 + col * cell_width + (cell_width - node.width) / 2
        y = zone.y + 50 + row * cell_height + (cell_height - node.height) / 2

        positions.append({"x": x, "y": y})

    return positions
```

### 4. Cross-Zone Connections

Calculate optimal edge paths between zones:

```
def calculate_edge_path(from_node, to_node):
    """Determine edge connection points"""
    # Calculate centers
    from_center = (from_node.x + from_node.width/2,
                   from_node.y + from_node.height/2)
    to_center = (to_node.x + to_node.width/2,
                 to_node.y + to_node.height/2)

    # Determine best sides to connect
    dx = to_center[0] - from_center[0]
    dy = to_center[1] - from_center[1]

    # Choose sides based on direction
    if abs(dx) > abs(dy):
        # Horizontal connection
        from_side = "right" if dx > 0 else "left"
        to_side = "left" if dx > 0 else "right"
    else:
        # Vertical connection
        from_side = "bottom" if dy > 0 else "top"
        to_side = "top" if dy > 0 else "bottom"

    return {
        "fromSide": from_side,
        "toSide": to_side
    }
```

## Advanced Techniques

### Force-Directed Layout

For complex networks with many cross-connections:

```
def force_directed_layout(nodes, edges, iterations=100):
    """Spring-based layout algorithm"""
    # Constants
    SPRING_LENGTH = 200
    SPRING_CONSTANT = 0.1
    REPULSION_CONSTANT = 5000

    for iteration in range(iterations):
        # Calculate repulsive forces (all pairs)
        for node1 in nodes:
            force_x, force_y = 0, 0

            for node2 in nodes:
                if node1 == node2:
                    continue

                dx = node1.x - node2.x
                dy = node1.y - node2.y
                distance = sqrt(dx*dx + dy*dy)

                if distance > 0:
                    # Repulsive force
                    force = REPULSION_CONSTANT / (distance * distance)
                    force_x += (dx / distance) * force
                    force_y += (dy / distance) * force

            node1.force_x = force_x
            node1.force_y = force_y

        # Calculate attractive forces (connected nodes)
        for edge in edges:
            node1 = get_node(edge.fromNode)
            node2 = get_node(edge.toNode)

            dx = node2.x - node1.x
            dy = node2.y - node1.y
            distance = sqrt(dx*dx + dy*dy)

            # Spring force
            force = SPRING_CONSTANT * (distance - SPRING_LENGTH)

            node1.force_x += (dx / distance) * force
            node1.force_y += (dy / distance) * force
            node2.force_x -= (dx / distance) * force
            node2.force_y -= (dy / distance) * force

        # Apply forces
        for node in nodes:
            node.x += node.force_x
            node.y += node.force_y
```

### Hierarchical Clustering

Group related nodes automatically:

```
def hierarchical_cluster(nodes, similarity_threshold=0.7):
    """Cluster nodes by content similarity"""
    clusters = []

    # Calculate similarity matrix
    similarity = calculate_similarity_matrix(nodes)

    # Agglomerative clustering
    current_clusters = [[node] for node in nodes]

    while len(current_clusters) > 1:
        # Find most similar clusters
        max_sim = 0
        merge_i, merge_j = 0, 1

        for i in range(len(current_clusters)):
            for j in range(i + 1, len(current_clusters)):
                sim = cluster_similarity(current_clusters[i],
                                        current_clusters[j],
                                        similarity)
                if sim > max_sim:
                    max_sim = sim
                    merge_i, merge_j = i, j

        if max_sim < similarity_threshold:
            break

        # Merge clusters
        current_clusters[merge_i].extend(current_clusters[merge_j])
        current_clusters.pop(merge_j)

    return current_clusters
```
