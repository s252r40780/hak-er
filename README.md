from pygame import *
from random import randint, choice
from time import time as timer
from webbrowser import *
from turtle import*

win_width = 1000
win_height = 700
window = display.set_mode((win_width, win_height))
display.set_caption('')
background = transform.scale(image.load('y.jpg'), (win_width, win_height))
background2 = transform.scale(image.load('final.png'), (win_width, win_height))
background3 = transform.scale(image.load('back123.jpg'), (win_width, win_height))
font.init()
font1 = font.SysFont('Arial', 15)
font2 = font.SysFont('Arial', 20)
font3 = font.SysFont("None", 50)
font4 = font.SysFont("None", 60)
mixer.init()

mixer.music.load('background.mp3')
mixer.music.play()
life = 100
fightmuz =mixer.Sound('fight.ogg')



# pLeft = [transform.scale(image.load("left/left"), ())]
hp1 = 50
game = True
finish = False
menu = True
fight = True
settings = False
background_changed = False
disurl = "https://discord.gg/W7vCRk4J"
pDown = [image.load("up.jpg"), image.load("up2.jpg")]
pLeft = [image.load("left.jpg"), image.load("left2.jpg"), image.load("left3.jpg")]
pRight = [image.load("right.jpg"), image.load("right2.jpg"), image.load("right3.jpg")]
pUp = [image.load("down.jpg"), image.load("down2.jpg"), image.load("down3.jpg")]


# Классы
class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed, w, h, hp):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (w, h))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))


class Player(GameSprite):
    def update(self):
        keys = key.get_pressed()
        if keys[K_LEFT]:
            self.rect.x -= self.speed
        if keys[K_RIGHT]:
            self.rect.x += self.speed
        if keys[K_UP]:
            self.rect.y -= self.speed
        if keys[K_DOWN]:
            self.rect.y += self.speed
        # if self.rect.left < 0 or self.rect.right > win_width or self.rect.top < 0 or self.rect.bottom > win_height:
        # global background_changed
        # if not background_changed:
        # window.blit(background2, (0, 0))
        # background_changed = True
        # self.rect.x = win_width // 2
        # self.rect.y = win_height // 2
        # else:
        # background_changed = False

    # def fire(self):
    # bullet = Bullet('kit.png', self.rect.centerx, self.rect.top, 20, 15, 20)
    # bullets.add(bullet)


class Enemy(GameSprite):
    def __init__(self, enemy_image, x, y, w, h):
        super().__init__(enemy_image, x, y, 0, w, h, 0)
        self.image = transform.scale(image.load(enemy_image), (w, h))
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y


class Enemy_fire(GameSprite):
    def update(self):
        global lost
        self.rect.y += self.speed
        if self.rect.y > win_height:
            self.rect.y = 0
            self.rect.x = randint(80, win_width - 80)
def Jakob_fight():
    player_x = 60
    player_y = 600
    warning = font2.render("1-обычная атака 2-всплеск огня ", True, (255,255,255),(0, 0, 0))
    background.blit(warning, (300,20))
    enemy1_h = font1.render("50хп макс", True, (255,255,255),(0, 0, 0))
    background.blit(enemy1_h, (60,590))
    fightmuz.play()
    
    while fight:
        
        for e in event.get():
            if e.type == QUIT:
                fight = False
                game = False
            if e.type == KEYDOWN:
                if e.key == K_1:
                    hp1 -= 25
                    life -=25
                    hpquart = font1.render("-25", True, (255,255,255),(0, 0, 0))
                    background.blit(hpquart, (60,590))
                    time.wait(2000)
                if e.key == K_2:
                    hp1 -=100
                    hpfull = font1.render("-100", True, (255,255,255),(0, 0, 0))
                    background.blit(hpfull, (60,590))
                    time.wait(2000)
                if hp1 <=0:
                    fight.stop()
                    fight = False
                

                



    
# Функции
def draw_menu():
    window.blit(background3, (0, 0))
    quest1 = font3.render(f"1 - Начать игру", True, (255, 255, 255))
    quest3 = font3.render(f"4 - Выбрать сложность", True, (255, 255, 255))
    quest2 = font3.render(f"2 - настройки", True, (255, 255, 255))
    opening = font4.render(f"Эльфийский квест: Волшебные миры", True, (46, 139, 87))
    discord = font3.render(f'3 - Дискорд', True, (255, 255, 255))
    bukva = font4.render(f"В", True, (255, 255, 255))
    text_rect = quest1.get_rect(center=(win_width // 2, win_height // 2))
    text_rect2 = quest2.get_rect(center=(500, 300))
    text_rect3 = opening.get_rect(center=(500, 250))
    text_rect4 = bukva.get_rect(center=(537.5, 250))
    text_rect5 = discord.get_rect(center=(500, 400))
    arch = transform.scale(image.load("luk.png"), (30, 30))
    arch2 = transform.scale(image.load("strela.png"), (32, 53))
    window.blit(quest1, text_rect)
    window.blit(quest2, text_rect2)
    window.blit(opening, text_rect3)
    window.blit(bukva, text_rect4)
    window.blit(arch, (789, 214))
    window.blit(arch2, (390, 224))
    window.blit(discord, text_rect5)
    window.blit(quest3, (320, 600))

    display.update()


def draw_difficult():
    # Фон
    window.blit(background, (0, 0))
    # Виды сложностей
    diff = font3.render(f'Сложность игры', True, (255, 255, 255))
    window.blit(diff, (500, 0))
    easy = font3.render(f'1 - Легко', True, (0, 255, 0))
    window.blit(easy, (320, 500))
    hard = font3.render(f'2 - Сложно', True, (255, 0, 0))
    window.blit(hard, (320, 600))
    impossible = font3.render(f'3 - Невозможно', True, (255, 255, 255))
    window.blit(impossible, (320, 550))
    if e.key == K_1:
        life = 100
    if e.key == K_2:
        life = 50
    if e.key == K_3:
        life = 1


def start_game():
    global game, menu
    game = True
    menu = False


def draw_back():
    window.blit(background2, (0, 0))


def draw_game():
    if background_changed:
        draw_back()
    else:
        pass


# def draw_settings():
#   window.blit(background3,(0,0))
#  quest1 = font3.render('1 - Выйти',True,(255,255,255))
# text_rect = quest1.get_rect(center=(win_width // 2, win_height // 2))
# window.blit(quest1, text_rect)
#
#   display.update()

# Цикл меню
while menu:
    for e in event.get():
        if e.type == QUIT:
            menu = False
            game = False
        if e.type == KEYDOWN:
            if e.key == K_1:
                start_game()
        if e.type == KEYDOWN:
            if e.key == K_3:
                open(disurl)
        if e.type == KEYDOWN:
            if e.key == K_4:
                draw_difficult()

                

        # if e.type == KEYDOWN:
        #   if e.key == K_2:
        #      draw_settings()
        #     menu = False
        #    settings = True

    # while settings:
    #   for e in event.get():
    #      if e.type == QUIT:
    #         settings = False
    #        game = False
    #   if e.type == KEYDOWN:
    #      if e.key == K_1:
    #         game = False
    #         settings = False

    draw_menu()

# Цикл настроек  

# Отрисовка Эльфов23
enemy1 = Enemy('elf.png', 50, 600, 75, 75)
enemy2 = Enemy('elf2.png', 800, 600, 110, 110)
# enemy2 = Enemy('zloi.png', 0, 0, 115, 130, )
player = Player('ii.png', 300, 440, 10, 50, 100, 100)

# Координаты x, y
x = 200
y = 600

player_x = player.rect.x
player_y = player.rect.y

# Игровой цикл
while game:

    for e in event.get():
        if e.type == QUIT:
            game = False
        if e.type == KEYDOWN:
            if e.key == K_DOWN:
                player.rect.y += 5
                pDown[0]
                pDown[1]
            if e.key == K_UP:
                player.rect.y -= 5
                pUp[0]
                pUp[1]
                pUp[2]
            if e.key == K_LEFT:
                player.rect.x -= 5
                pLeft[0]
                pLeft[1]
                pLeft[2]
            if e.key == K_RIGHT:
                player.rect.x += 5
                pRight[0]
                pRight[1]
                pRight[2]
    player_x = player.rect.x
    player_y = player.rect.y

    if not finish:
        draw_back()
        window.blit(background, (0, 0))
        player.reset()
        player.update()
        enemy1.reset()
        enemy1.update()
        enemy2.reset()
        enemy2.update()
        keys = key.get_pressed()

        enemy3 = transform.scale(image.load("elf.png"), (75, 75))
        window.blit(enemy3, (700, 400))
        warning = font2.render("--Читайте подсказки!", True, (255,255,255),(0, 0, 0))
        background.blit(warning, (300,20))

        if player_x < 300:
            quest2 = font1.render("Спросить Джейкоба - q, напасть - 1", True, (255,255,255),(0, 0, 0))
            background.blit(quest2, (0,0))
        if player_x > 400:
            quest2 = font1.render("Спросить Луну - w напасть - 2", True, (255,255,255),(0, 0, 0))
            background.blit(quest2, (0,20))
        

        # Отображение надписи на экране
        if keys[K_1]:
            Jakob_fight()

        if keys[K_q]:
            if player_x<400:
                answ = font1.render(
                f'я - Джейкоб и мы сейчас находимся на тропинке к замку, если ты туда идешь то иди на право', True,(255, 255, 255), (0, 0, 0))
                background.blit(answ, (60, 590))
          



        if keys[K_w]:
            answ2 = font1.render(f'''Меня зовут Луна, и я - эльф, если ты идешь к замку то удачи''', True,(255, 255, 255), (0, 0, 0))
            answ3 = font1.render(
                f'''Вокруг нас воцарилась тишина, только слабый ветер шепчет свои таинственные истории...''', True,
                (255, 255, 255), (0, 0, 0))
            background.blit(answ2, (420, 380))

            background.blit(answ3, (450, 570))

            answ2 = font1.render(f'', 1, (255, 255, 255), (0, 0, 0))

            

            display.update()
        

        if keys[K_3]:
            background_changed = True
            background2.blit(player)
            # window.blit(background2, (0,0))

        

        if life >= 70:
            life_text_color = (0, 255, 0)
        if 20 <= life < 70:
            life_text_color = (250, 200, 50)
        if life <= 0 or life < 20:
            life_text_color = (255, 0, 0)
        if life <= 0:
            finish = True
            window.blit(lose, (250, 250))
        life_text = font2.render(f'Здоровье:{life}', True, life_text_color, (0, 0, 0))
        background.blit(life_text, (850, 10))

        # window.blit(background, (0, 0))

        # enemy2.reset()
        # enemy2.update()

        draw_game()
        display.flip()

    time.delay(5)
    display.update()
