# Simple-Pendulum

from Pendulum import Pendulum 
from vpython import *


p = Pendulum()
print(p)
print(p.calculateAngularAcceleration())
p.update(0.1)
print(p.calculatekineticEnergy())
p.updatekineticEnergy(0.1)
print(p)


graph(x=200,y=400)
fdvstime = gcurve(color=color.white)

## Initialize run
time     = 0.0
tmax     = 50
dt       = 0.005

graph(x=500,y=400,title = 'thetadot vs. theta')
thetadotvstheta = gcurve(color=color.green)


graph(x=0,y=0,title = 'kinetic(green) and potential(red) energy vs. time')
kineticvstime = gcurve(color=color.green)
potentialvstime = gcurve(color=color.red)

graph(x=0,y=0,title = 'theta vs. time')
thetavstime = gcurve(color=color.green)

canvas(x=800,y=0)
bob = sphere(pos=vector(p.length*sin(p.theta),-p.length*cos(p.theta),0), radius = p.length/10.0)
rod = cylinder(pos=vector(0,0,0),axis=bob.pos,radius=bob.radius*0.1)
# Reference line.
rod0 = cylinder(pos=vector(0,0,0),axis=bob.pos,radius=bob.radius*0.1, color=color.red, opacity = 0.25) 

while time < tmax:
    rate(100)
    p.update(dt)
    p.updatekineticEnergy(dt)
    p.updatepotentialEnergy(dt)
    time +=dt
    thetadotvstheta.plot(pos=(p.theta, p.angularVelocity))
    thetavstime.plot(pos=(time, p.theta))
    
    kineticvstime.plot(pos=(time, p.kineticEnergy))
    potentialvstime.plot(pos=(time, p.potentialEnergy))
    bob.pos  = vector(p.length*sin(p.theta),-p.length*cos(p.theta),0)
    rod.axis = bob.pos
    
