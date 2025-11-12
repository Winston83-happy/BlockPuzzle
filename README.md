# BlockPuzzle
This is a simple block-sliding puzzle made in Godot using GDScript.
extends Node2D

@export var grid_size_x: int = 5
@export var grid_size_y: int = 5
@export var tile_scene: PackedScene # Scene for individual block tiles

var grid: Array[Array] # 2D array to store references to block nodes
var selected_block: Sprite2D = null

func _ready():
	_initialize_grid()
	_populate_grid()

func _initialize_grid():
	# Create an empty grid
	for i in range(grid_size_x):
		grid.append([])
		for j in range(grid_size_y):
			grid[i].append(null)

func _populate_grid():
	# Instantiate and place block tiles
	for x in range(grid_size_x):
		for y in range(grid_size_y):
			var new_tile = tile_scene.instantiate()
			add_child(new_tile)
			new_tile.position = Vector2(x * 64, y * 64) # Assuming 64x64 pixel tiles
			new_tile.grid_position = Vector2i(x, y) # Custom property to store grid position
			grid[x][y] = new_tile
			# Assign a random color or type to the tile for basic puzzle logic
			# new_tile.modulate = Color(randf(), randf(), randf())

func _input(event):
	if event is InputEventMouseButton and event.pressed and event.button_index == MOUSE_BUTTON_LEFT:
		var mouse_pos = get_global_mouse_position()
		for x in range(grid_size_x):
			for y in range(grid_size_y):
				var tile = grid[x][y]
				if tile and tile.get_global_rect().has_point(mouse_pos):
					_handle_tile_click(tile)
					return

func _handle_tile_click(clicked_tile: Sprite2D):
	if selected_block == null:
		selected_block = clicked_tile
		# Visual feedback for selection, e.g., change color or scale
		selected_block.modulate = Color.YELLOW
	elif selected_block == clicked_tile:
		# Deselect if clicking the same block again
		selected_block.modulate = Color.WHITE
		selected_block = null
	else:
		_swap_blocks(selected_block, clicked_tile)
		selected_block.modulate = Color.WHITE
		selected_block = null
		_check_for_matches() # Or other puzzle-solving logic

func _swap_blocks(block1: Sprite2D, block2: Sprite2D):
	var pos1 = block1.grid_position
	var pos2 = block2.grid_position

	# Update grid references
	grid[pos1.x][pos1.y] = block2
	grid[pos2.x][pos2.y] = block1

	# Swap visual positions and update grid_position property
	var temp_pos = block1.position
	block1.position = block2.position
	block2.position = temp_pos

	block1.grid_position = pos2
	block2.grid_position = pos1

func _check_for_matches():
	# Implement your puzzle-solving logic here
	# For example, iterate through the grid and check for 3 or more
	# consecutive blocks of the same color/type.
	# If matches are found, remove them, shift blocks, and refill.
	print("Checking for matches...")
	# Example: Check for horizontal matches
	for y in range(grid_size_y):
		for x in range(grid_size_x - 2):
			var block1 = grid[x][y]
			var block2 = grid[x+1][y]
			var block3 = grid[x+2][y]
			# Assuming blocks have a 'type' property

By:Winston Deng 11/5/25
			# if block1.type == block2.type and block2.type == block3.type:
			# 	print("Match found at:", x, y)
			# 	# Handle match (e.g., remove blocks, trigger animation)   
