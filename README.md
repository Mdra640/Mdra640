import pygame
import time
from speech_recognition import Recognizer, Microphone

# Initialize Pygame
pygame.init()
screen = pygame.display.set_mode((500, 500))

# Initialize speech recognizer
recognizer = Recognizer()

def listen_to_voice():
    with Microphone() as source:
        print("Listening for commands...")
        audio = recognizer.listen(source)
    try:
        command = recognizer.recognize_google(audio)
        print(f"Recognized: {command}")
        return command
    except:
        print("Could not understand audio")
        return None

# Circle properties
x, y = 250, 250
radius = 50
speed = 10

# Main loop
running = True
while running:
    screen.fill((0, 0, 0))  # Black background

    command = listen_to_voice()
    if command and "move" in command.lower():
        x += speed
        if x > 500:
            x = 0

    # Draw circle
    pygame.draw.circle(screen, (255, 0, 0), (x, y), radius)
    pygame.display.flip()
    
    time.sleep(1)

    # Handle quit event
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

pygame.quit()

