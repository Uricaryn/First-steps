# First-steps
This is gonna be the first chapter of my coding journey

import pygame
import time
import random

# Initialize Pygame
pygame.init()

# Colors
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)

# Display settings
width, height = 800, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption('Snake Game')

# Snake settings
snake_block = 20
snake_speed = 15

# Clock to control the game speed
clock = pygame.time.Clock()

# Font for displaying the score
font = pygame.font.Font(None, 36)

def message(text, color):
    msg = font.render(text, True, color)
    screen.blit(msg, [width / 2, height / 2])

def gameLoop():
    # Initialize the snake
    snake = [[width / 2, height / 2]]
    snake_direction = 'RIGHT'
    snake_length = 1

    # Initialize the food
    food_x = round(random.randrange(0, width - snake_block) / snake_block) * snake_block
    food_y = round(random.randrange(0, height - snake_block) / snake_block) * snake_block

    game_over = False

    while not game_over:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT and snake_direction != 'RIGHT':
                    snake_direction = 'LEFT'
                if event.key == pygame.K_RIGHT and snake_direction != 'LEFT':
                    snake_direction = 'RIGHT'
                if event.key == pygame.K_UP and snake_direction != 'DOWN':
                    snake_direction = 'UP'
                if event.key == pygame.K_DOWN and snake_direction != 'UP':
                    snake_direction = 'DOWN'

        if snake[0][0] >= width or snake[0][0] < 0 or snake[0][1] >= height or snake[0][1] < 0:
            game_over = True

        for block in snake[1:]:
            if snake[0] == block:
                game_over = True

        # Move the snake
        if snake_direction == 'RIGHT':
            snake_head = [snake[0][0] + snake_block, snake[0][1]]
        if snake_direction == 'LEFT':
            snake_head = [snake[0][0] - snake_block, snake[0][1]]
        if snake_direction == 'UP':
            snake_head = [snake[0][0], snake[0][1] - snake_block]
        if snake_direction == 'DOWN':
            snake_head = [snake[0][0], snake[0][1] + snake_block]

        snake.insert(0, snake_head)

        # Check if the snake eats the food
        if snake[0][0] == food_x and snake[0][1] == food_y:
            food_x = round(random.randrange(0, width - snake_block) / snake_block) * snake_block
            food_y = round(random.randrange(0, height - snake_block) / snake_block) * snake_block
            snake_length += 1

        while len(snake) > snake_length:
            snake.pop()

        screen.fill(black)
        for segment in snake:
            pygame.draw.rect(screen, white, [segment[0], segment[1], snake_block, snake_block])
        pygame.draw.rect(screen, red, [food_x, food_y, snake_block, snake_block])

        pygame.display.update()
        clock.tick(snake_speed)

    pygame.quit()
    quit()

gameLoop()
