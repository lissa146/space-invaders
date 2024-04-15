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

class wall:
    def __init__(self,xpos,ypos):
        self.xpos = xpos
        self.ypos = ypos
        self.numHits = 0
        self.isAlive = True
    
    def draw(self):
        if self.isAlive:
           pygame.draw.rect(screen, (255, 153, 204), (20, 20, 20, 20))
#-------------------------------------------------------------------
#make one wall for testing
oneWall = wall(200, 300)

#make a bunch of walls
walls = []
for k in range (4):
    for i in range (2):
        for j in range (3):
            walls.append(wall(j*30+200*k+50, i*30+600))
            
            
            
#bullet class--------------------------------------------
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
#end bullet class--------------------------------------------        

#create (aka instantiate) a bullet
bullet = Bullet(xpos+28, ypos)               

class Alien:
    def __init__(self, xpos, ypos):
        self.xpos = xpos
        self.ypos = ypos
        self.isAlive = True
        self.direction = 1
    
    def draw(self):
        if self.isAlive:
            pygame.draw.rect(screen, (250, 250, 250), (self.xpos, self.ypos, 40, 40))
    
    def move(self, time):
        if time % 800 == 0:
            self.ypos += 100
            self.direction *=-1
            return 0
        
        if time % 100 == 0:
            self.xpos+=50*self.direction
        
        return time
    
    def collide(self, bullet):
        BulletX = bullet.xpos
        BulletY = bullet.ypos
        if self.isAlive:
            if BulletX > self.xpos:
                if BulletX < self.xpos + 40:
                    if BulletY < self.ypos + 40:
                        if BulletY > self.ypos:
                            print("hit!")
                            self.isAlive = False
                            return False
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
        bullet.move(xpos+28, ypos)
        if bullet.isAlive == True:
            
            for i in range (len(armada)):
                bullet.isAlive = armada[i].collide(bullet)
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
    for k in range (len(walls)):
        walls[k].draw()
    bullet.draw()
    pygame.display.flip()
    
#end of game loop
pygame.quit()
