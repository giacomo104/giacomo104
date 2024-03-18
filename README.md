import pygame
import random

# Inizializzazione di Pygame
pygame.init()

# Definizione di alcuni colori
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Impostazione delle dimensioni dello schermo
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Subway Surfer")

# Definizione della classe per il giocatore
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([50, 50])
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.x = 50
        self.rect.y = SCREEN_HEIGHT // 2
        self.velocity = 0

    def update(self):
        self.rect.y += self.velocity

# Definizione della classe per gli ostacoli
class Obstacle(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([50, 50])
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.x = SCREEN_WIDTH
        self.rect.y = random.randrange(SCREEN_HEIGHT - 50)
        self.speed = random.randrange(1, 4)

    def update(self):
        self.rect.x -= self.speed

# Creazione di gruppi per gestire sprite
all_sprites_group = pygame.sprite.Group()
obstacles_group = pygame.sprite.Group()

# Creazione del giocatore
player = Player()
all_sprites_group.add(player)

# Variabili per il gioco
clock = pygame.time.Clock()
score = 0
game_over = False

# Loop di gioco
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                player.velocity = -5

    # Generazione degli ostacoli
    if random.randrange(100) < 2:
        obstacle = Obstacle()
        all_sprites_group.add(obstacle)
        obstacles_group.add(obstacle)

    # Collisione tra il giocatore e gli ostacoli
    obstacles_hit = pygame.sprite.spritecollide(player, obstacles_group, False)
    if obstacles_hit:
        game_over = True

    # Aggiornamento del giocatore e degli ostacoli
    all_sprites_group.update()

    # Pulizia dello schermo
    screen.fill(BLACK)

    # Disegno degli sprite
    all_sprites_group.draw(screen)

    # Aggiornamento dello schermo
    pygame.display.flip()

    # Impostazione del frame rate
    clock.tick(60)

# Terminazione di Pygame
pygame.quit()
