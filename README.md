# 123qwe
```python
from pygame import *
mixer.init()
mixer.music.load('jungles.ogg')
mixer.music.play()
class GameSprite(sprite.Sprite):
   def __init__(self, player_image, player_x, player_y, player_speed):
       super().__init__()
       self.image = transform.scale(image.load(player_image), (65, 65))
       self.speed = player_speed
       self.rect = self.image.get_rect()
       self.rect.x = player_x
       self.rect.y = player_y

   def reset(self):
       window.blit(self.image, (self.rect.x, self.rect.y))
class Player(GameSprite):
    def update(self):
        keys = key.get_pressed()
        if keys[K_LEFT] and self.rect.x > 5:
            self.rect.x -= self.speed
        if keys[K_RIGHT] and self.rect.x < win_width - 80:
            self.rect.x += self.speed
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < win_height - 80:
            self.rect.y += self.speed
class Enemy(GameSprite):
    def update(self):
        if self.rect.x <= 470:
            self.direction = 'right'
        if self.rect.x >= win_width - 85:
            self.direction = 'left' 

        if self.direction == 'left':
            self.rect.x -= self.speed
        else:
            self.rect.x += self.speed
class Wall(sprite.Sprite):
    def __init__(self, color_1, color_2, color_3, wall_x, wall_y, wall_width, wall_height):
        super().__init__()
        self.color_1 = color_1
        self.color_2 = color_2
        self.color_3 = color_3
        self.width = wall_width
        self.height = wall_height
        self.image = Surface((self.width, self.height))
        self.image.fill((color_1, color_2, color_3))
        self.rect = self.image.get_rect()
        self.rect.x = wall_x
        self.rect.y = wall_y
    def draw_wall(self):
        window.blit(self.image, (self.rect.x, self.rect.y))
kick = mixer.Sound('kick.ogg')
kick.play()
FPS = 60
clock = time.Clock()
win_width = 700
win_height = 500
packman = Player('hero.png', 100, 100, 4)
window = display.set_mode((win_width, win_height))
display.set_caption('Maze')
monster = Enemy('cyborg.png', 100, 100, 4)
background = transform.scale(image.load('background.jpg'), (win_width, win_height))
game = True
finish = False
display.update()
sp1 = Player('hero.png', 75, 75, 4)
sp2 = Enemy('cyborg.png', 75, 75, 2)
wall1 = Wall(154, 205, 50, 300, 20, 450, 10)
wall2 = Wall(154, 205, 50, 300, 480, 450, 10)
wall3 = Wall(154, 205, 50, 20, 100, 10, 300)
while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
    if finish != True:
        if sprite.collide_rect(packman, monster) or sprite.collide_rect(packman, wall1) or sprite.collide_rect(packman, wall2) or sprite.collide_rect(packman, wall3):
            finish = True
            kick.play()
            time.delay(1000)
            packman.rect.x = 100
            packman.rect.y = 100
        elif sprite.collide_rect(packman, final):
            finish = True
            money.play()
    window.blit(background, (0, 0))
    packman.update()
    packman.reset()
    monster.update()
    monster.reset()
    wall1.draw_wall()
    wall2.draw_wall()
    wall3.draw_wall()
    display.update()
    clock.tick(FPS)
```
