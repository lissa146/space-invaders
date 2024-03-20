# space-invaders
import pygame

pygame.init()
pygame.display.set_caption("space invaders")
screen = pygame.display.set_mode((800,800))
clock = pygame.time.Clock()
gameover = False
xpos = 400
ypos = 750
moveLeft = False
moveRight = False
timer = 0
shoot = False
class Bullet:
    def __init__(self,xpos, ypos):
        self.xpos = xpos
        self.ypos = ypos
        self.isAlive = False
    def move(self, xpos, ypos):
        if self.isAlive == True:
            self.ypos-=5
        if self.ypos < 0:
            self.IsAlive = False
            self.xpos = xpos
            self.ypos = ypos
    def draw(self):
        pygame.draw.rect(screen, (250, 250, 250), (self.xpos, self.ypos, 3, 20))
bullet = Bullet(xpos+28, ypos)               

class Alien:
    def __init__(self, xpos, ypos):
        self.xpos = xpos
        self.ypos = ypos
        self.isAlive = True
        self.direction = 1
    def draw(self):
        if self.isAlive = True:
            pygame.draw.rect(screen, (250, 250, 250), (self.xpos, self.ypos, 40, 40))
        else:
            self.isAlive
    def move(self, time):
        if time % 800 == 0:
            self.ypos += 100
            self.direction *=-1
            return 0
        
        if time % 100 == 0:
            self.xpos+=50*self.direction
        
        return time
    
    def collide(self, BulletX, BulletY):
        if self.isAlive: #only hit live aliens 
            if BulletX > self.xpos:#check if bullet is right of the left side of the alien 
                if BulletX < self.xpos + 40:#check if the bullet is loeft of the right side 
                    if BulletY < self.ypos + 40:#check if the bullet is above the aliens bottom 
                        if BulletY > self.ypos:#check if the bullet is below the top of the alien 
                            print("hit!")#for testing
                            self.isAlive = False#set the alien to dead
                            return False#set the bu
        return True
                
        
armada = []
for i in range (4):
    for j in range (9):
        armada.append(Alien(j*80+50, i*80+50))


while not gameover:
    clock.tick(60)#FPS
    timer += 1
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            gameover = True
        
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                moveLeft = True
            if event.key == pygame.K_RIGHT:
                moveRight = True
            if event.key == pygame.K_SPACE:
                shoot = True
                print("shoot")

        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT:
                moveLeft = False
            if event.key == pygame.K_RIGHT:
                moveRight = False
            if event.key == pygame.K_SPACE:
                shoot = False
        
    ##pysicasl

    if moveLeft == True:
        vx = -3
    elif moveRight == True:
        vx = 3
    else:
        vx = 0
            
    xpos += vx
    for i in range (len(armada)):
        timer = armada[i].move(timer)
    
    if shoot == True:
        bullet.isAlive = True
    
    if bullet.isAlive == True:
        bullet.move(xpos+28, ypos) #shoot from player  
        if bullet.isAlive == True:
            #check for collions between bullet and ememy
            for i in range (len(armada)):#check bullet with entire armada's positions
                bullet.isAlive = armada[i].collide(bullet.xpos, bullet.ypos) #if we hit, set bullet to falkse
                if bullet.isAlive == False:
                    break   
    else:
        bullet.xpos = xpos +28
        bullet.ypos = ypos
        
    #RENDER SECTION
    screen.fill((0,0,0))
    
    pygame.draw.rect(screen, (200, 200, 100), (xpos, 750, 60, 20))
    pygame.draw.rect(screen, (200, 200, 100), (xpos+5, 746, 100/2, 5))
    pygame.draw.rect(screen, (200, 200, 100), (xpos+22, 738, 15, 15))
    pygame.draw.rect(screen, (200, 200, 100), (xpos+26, 734, 5, 5))
    for i in range (len(armada)):
        armada[i].draw()
    bullet.draw()
    pygame.display.flip()
    
#end of game loop
pygame.quit()
