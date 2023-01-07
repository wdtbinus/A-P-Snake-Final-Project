# Snake Game Project Documentation

**This final project is centered around the game Snake. The objective in this game is to catch the maximum number of food without colliding yourself (as the snake).**

**Here's a screenshot of the game in action:**

![Application Screenshot](https://github.com/wdtbinus/A-P-Snake-Final-Project/blob/main/Resources/Application%20Screenshot.png?raw=true)

**This documentation will break down the game's code (top to bottom), providing context and reason for each component used for the game, including all the essential algorithms within this program.**

**At the end of this documentation, you will be able to read the full source code for the project.**

**For this program, you will need to have Python and pip installed on your computer (generally, when Python is installed, pip should have also been installed as well). Through pip, you will need to install the *turtle* and *random* modules.**

**The code below imports the *turtle* and *random* module. The *turtle* module enables users to create visuals through a virtual canvas and the *random* module generates random numbers (this will come in handy especially with Food).**

Start an indented code block following a paragraph with a blank line and at least four spaces of indentation:

    import turtle
    import random

**Below, values for Screen width and height, Food size, and Snake's movement delay and time are defined:**

    w = 500
    h = 500
    food_size = 10
    delay = 100

**Below, offsets of Snake's movement in each direction are defined:**

    offsets = {
        "up": (0, 20),
        "down": (0, -20),
        "left": (-20, 0),
        "right": (20, 0)
    }

**Snake is assigned with an initial list, first starting from the tail and ending with the head, allowing it to be easier to append a new head in the list and pop the first item (the end of the tail). Snake spawns with initial movement upwards. Food's position is randomly generated and visualized within that position. See below:**

    def reset():
        global snake, snake_dir, food_position, pen
        snake = [[0, 0], [0, 20], [0, 40], [0, 60], [0, 80]]
        snake_dir = "up"
        food_position = get_random_food_position()
        food.goto(food_position)
        move_snake()

**List items are shifted around based on Snake's movement. It is also ensured that if Snake's head collides with another part of Snake, the game resets, otherwise, a new head is appended, and if Snake's head doesn't collide with Food, the first item in the list gets popped. See below:**

    def move_snake():
        global snake_dir

        new_head = snake[-1].copy()
        new_head[0] = snake[-1][0] + offsets[snake_dir][0]
        new_head[1] = snake[-1][1] + offsets[snake_dir][1]

        if new_head in snake[:-1]:
            reset()
        else:
            snake.append(new_head)

            if not food_collision():
	            snake.pop(0)

**It is ensured that if Snake passes through one side of the screen, Snake comes out from the opposite side, this creates no barrier in the game. Snake is also visualized through stamps and Snake is restamped through each movement, Screen updates to visualize Snake's movement. See below:**

    if snake[-1][0] > w / 2:
        snake[-1][0] -= w
    elif snake[-1][0] < - w / 2:
	    snake[-1][0] += w
	elif snake[-1][1] > h / 2:
		snake[-1][1] -= h
	elif snake[-1][1] < -h / 2:
		snake[-1][1] += h

    pen.clearstamps()

    for segment in snake:
	    pen.goto(segment[0], segment[1])
	    pen.stamp()

	screen.update()

	turtle.ontimer(move_snake, delay)

**It is ensured that if Snake's head is under 20 pixels from Food, Food is considered collided with (eaten by Snake), otherwise, if Snake is further than that distance, Food isn't collided with. See below:**

    def food_collision():
        global food_position
        if get_distance(snake[-1], food_position) < 20:
	        food_position = get_random_food_position()
	        food.goto(food_position)
	        return True
	    return False

**Below, positions for the next Food is generated:**

    def get_random_food_position():
        x = random.randint(- w / 2 + food_size, w / 2 - food_size)
        y = random.randint(- h / 2 + food_size, h / 2 - food_size)
        return (x, y)

**Below, distance between Snake and Food is calculated:**

    def get_distance(pos1, pos2):
        x1, y1 = pos1
        x2, y2 = pos2
        distance = ((y2 - y1) ** 2 + (x2 - x1) ** 2) ** 0.5
        return distance

**Below, the movement controls are defined:**

    def go_up():
        global snake_dir
        if snake_dir != "down":
	        snake_dir = "up"

    def go_right():
        global snake_dir
        if snake_dir != "left":
	        snake_dir = "right"

    def go_down():
        global snake_dir
        if snake_dir != "up":
	        snake_dir = "down"

    def go_left():
        global snake_dir
        if snake_dir != "right":
	        snake_dir = "left"

**Below, Screen traits, including things like Screen's title, setup, and background color, are defined:**

    screen = turtle.Screen()
    screen.setup(w, h)
    screen.title("Snake")
    screen.bgcolor("green")
    screen.setup(500, 500)
    screen.tracer(0)

    pen = turtle.Turtle("square")
    pen.penup()

**Below, Food traits, including things like Food's shape, color, and shape size, are defined:**

    food = turtle.Turtle()
    food.shape("square")
    food.color("red")
    food.shapesize(food_size / 20)
    food.penup()

**Below, the movement controls are bound:**

    screen.listen()
    screen.onkey(go_up, "Up")
    screen.onkey(go_right, "Right")
    screen.onkey(go_down, "Down")
    screen.onkey(go_left, "Left")

    reset()
    turtle.done()

**Here are the use case and activity diagrams for this project, summarizing details of the actors, system, and activities:**

Snake Use Case Diagram:

![Snake Use Case Diagram](https://github.com/wdtbinus/A-P-Snake-Final-Project/blob/main/Resources/Snake%20Use%20Case%20Diagram.png?raw=true)

Snake Activity Diagram:

![Snake Activity Diagram](https://github.com/wdtbinus/A-P-Snake-Final-Project/blob/main/Resources/Snake%20Activity%20Diagram.png?raw=true)

**Here is a full view of the source code (including comments):**

    # Imports the turtle and random module.

    import turtle
    import random

    # Defines screen width, height, Food size, and Snake's movement delay.

    w = 500
    h = 500
    food_size = 10
    delay = 100

    # Defines offsets of Snake's movement.

    offsets = {
        "up": (0, 20),
        "down": (0, -20),
        "left": (-20, 0),
        "right": (20, 0)
    }

    # Spawns Snake containing the given list (starting from tail to head).
    # Snake spawns with initial movement upwards.
    # Food's position is randomly generated and visualized within that position.

    def reset():
        global snake, snake_dir, food_position, pen
        snake = [[0, 0], [0, 20], [0, 40], [0, 60], [0, 80]]
        snake_dir = "up"
        food_position = get_random_food_position()
        food.goto(food_position)
        move_snake()

    # Shifts list items based on Snake's movement.
    # If Snake's head collides with another part of Snake, the game resets, otherwise, a new head is appended.
    # If Snake's head doesn't collide with Food, the first item in the list (the end of the tail) gets popped.

    def move_snake():
        global snake_dir

        new_head = snake[-1].copy()
        new_head[0] = snake[-1][0] + offsets[snake_dir][0]
        new_head[1] = snake[-1][1] + offsets[snake_dir][1]

        if new_head in snake[:-1]:
            reset()
        else:
            snake.append(new_head)

            if not food_collision():
	            snake.pop(0)

	# Ensures that if Snake passes through one side of the screen, Snake comes out from the opposite side.
	# Snake is visualized through stamps, Snake is restamped through each movement.
	# Updates Screen to visualize Snake's movement.

    if snake[-1][0] > w / 2:
        snake[-1][0] -= w
    elif snake[-1][0] < - w / 2:
	    snake[-1][0] += w
	elif snake[-1][1] > h / 2:
		snake[-1][1] -= h
	elif snake[-1][1] < -h / 2:
		snake[-1][1] += h

    pen.clearstamps()

    for segment in snake:
	    pen.goto(segment[0], segment[1])
	    pen.stamp()

	screen.update()

	turtle.ontimer(move_snake, delay)

	# If Snake's head is under 20 pixels from Food, Food is considered collided with, otherwise, Food isn't collided with.

    def food_collision():
        global food_position
        if get_distance(snake[-1], food_position) < 20:
	        food_position = get_random_food_position()
	        food.goto(food_position)
	        return True
	    return False

	# Generates position for the next Food.

    def get_random_food_position():
        x = random.randint(- w / 2 + food_size, w / 2 - food_size)
        y = random.randint(- h / 2 + food_size, h / 2 - food_size)
        return (x, y)

    # Calculates distance between Snake and Food.

    def get_distance(pos1, pos2):
        x1, y1 = pos1
        x2, y2 = pos2
        distance = ((y2 - y1) ** 2 + (x2 - x1) ** 2) ** 0.5
        return distance

    # Defines movement controls.

    def go_up():
        global snake_dir
        if snake_dir != "down":
	        snake_dir = "up"

    def go_right():
        global snake_dir
        if snake_dir != "left":
	        snake_dir = "right"

    def go_down():
        global snake_dir
        if snake_dir != "up":
	        snake_dir = "down"

    def go_left():
        global snake_dir
        if snake_dir != "right":
	        snake_dir = "left"

	# Defines Screen traits.

    screen = turtle.Screen()
    screen.setup(w, h)
    screen.title("Snake")
    screen.bgcolor("green")
    screen.setup(500, 500)
    screen.tracer(0)

    pen = turtle.Turtle("square")
    pen.penup()

    # Defines Food traits.

    food = turtle.Turtle()
    food.shape("square")
    food.color("red")
    food.shapesize(food_size / 20)
    food.penup()

    # Binds movement controls.

    screen.listen()
    screen.onkey(go_up, "Up")
    screen.onkey(go_right, "Right")
    screen.onkey(go_down, "Down")
    screen.onkey(go_left, "Left")

    reset()
    turtle.done()