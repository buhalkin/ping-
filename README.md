# ping-
ping pong
from pygame import *
from time import monotonic
clock = time.Clock()
w_w = 1500
w_h = 900
run = True
g = 0
p1 = 0
p2 = 0
window = display.set_mode((w_w,w_h))
display.set_caption("Пинг-Понг")
background = transform.scale(image.load("neon.jpg"),(w_w,w_h))
player_x = 17
player_y = 150
ball_x = 5
ball_y = 5
spedd = 5
finish = True
font.init()
font1 = font.Font(None,100)
lose1 = font1.render("PLAYER 1 LOSE",True,(255,0,0))
lose2 = font1.render("PLAYER 2 LOSE",True,(255,0,0))
score1 = font1.render("score 1: "+str(p1),True,(255,0,0))
score2 = font1.render("score 2: "+str(p2),True,(255,0,0))
class GameSprite(sprite.Sprite):
    def __init__(self, play_image, play_x, play_y, play_speed):
        super().__init__()
        self.image = transform.scale(image.load(play_image), (64,48))
        self.speed = play_speed
        self.rect = self.image.get_rect()
        self.rect.x = play_x
        self.rect.y = play_y
    def reset(self):
        window.blit((self.image), (self.rect.x, self.rect.y))


class Rocket(sprite.Sprite):
    def __init__(self,weight,height,pos_x,pos_y,colour,):
        super().__init__()
        self.image = Surface((weight,height))
        self.image.fill(colour)
        self.rect = self.image.get_rect()
        self.rect.x = pos_x
        self.rect.y = pos_y
    def reset(self):
        window.blit((self.image), (self.rect.x, self.rect.y))
play1 = Rocket(player_x+5,player_y+5, 1/10*w_w-player_x, w_h/2-player_y/2,(0,255,255)) 
play2 = Rocket(player_x+5,player_y+5, 9/10*w_w, w_h/2-player_y/2,(255,0,255))
ball = GameSprite("ball.png",w_w/2-32,w_h/2-24,spedd)  
while run:
    window.blit(background,(0,0))
    play1.reset()
    play2.reset()
    ball.reset()
    window.blit(score2,(w_w-400,0))
    window.blit(score1,(0,0))
    if finish == True:
        key_key = key.get_pressed()
        if key_key[K_w] and play1.rect.y > 0:          
            play1.rect.y -= 10         
        if key_key[K_s] and play1.rect.y < w_h - player_y:           
            play1.rect.y += 10
        if key_key[K_UP] and play2.rect.y > 0:          
            play2.rect.y -= 10         
        if key_key[K_DOWN] and play2.rect.y < w_h - player_y:           
            play2.rect.y += 10
        ball.rect.x += ball_x
        ball.rect.y += ball_y
        if ball.rect.y < 0 or ball.rect.y > w_h - 48: 
            ball_y = -1 * ball_y
        if sprite.collide_rect(play1,ball):
            ball_x = -1 * ball_x
            p1 += 1
            score1 = font1.render("score 1: "+str(p2),True,(255,0,0))
            window.blit(score1,(0,0))
        if p1 and p2 >= 5:
            spedd = 10
        if p1 and p2 >= 10:
            spedd = 20
        if sprite.collide_rect(play2,ball):
            ball_x = -1 * ball_x
            p2 += 1
            score2 = font1.render("score 2: "+str(p2),True,(255,0,0))
            window.blit(score2,(w_w-400,0))
        if ball.rect.x < 0:
            window.blit(lose1,(w_w/2-250,w_h/2))
            g += 1
            if g > 300:
                play1 = Rocket(player_x+5,player_y+5, 1/10*w_w-player_x, w_h/2-player_y/2,(0,255,255)) 
                play2 = Rocket(player_x+5,player_y+5, 9/10*w_w, w_h/2-player_y/2,(255,0,255))
                ball = GameSprite("ball.png",w_w/2-32,w_h/2-24,5) 
                g = 0
                p1 = 0
                p2 = 0
                score1 = font1.render("score 1: "+str(p1),True,(255,0,0))
                score2 = font1.render("score 2: "+str(p2),True,(255,0,0))
        if ball.rect.x > w_w - 64:
            window.blit(lose2,(w_w/2-250,w_h/2))
            
            g += 1    
        if g > 300:
            play1 = Rocket(player_x+5,player_y+5, 1/10*w_w-player_x, w_h/2-player_y/2,(0,255,255)) 
            play2 = Rocket(player_x+5,player_y+5, 9/10*w_w, w_h/2-player_y/2,(255,0,255))
            ball = GameSprite("ball.png",w_w/2-32,w_h/2-24,5) 
            g = 0
            p1 = 0
            p2 = 0
            score1 = font1.render("score 1: "+str(p1),True,(255,0,0))
            score2 = font1.render("score 2: "+str(p2),True,(255,0,0))
             
    for e in event.get():
        if e.type == QUIT:          
            run = False
        if e.type == KEYDOWN:
            if e.key == K_F10:
                window = display.set_mode((w_w,w_h))
            if e.key == K_F11:
                window = display.set_mode((w_w,w_h),FULLSCREEN) 
    clock.tick(60)
    display.update()
