# Simple-Pendulum

import scipy.constants
import numpy 
import math

class Pendulum:
    """ Models a simple pendum"""

    def __init__(self, inputMass = 1.0, inputLength= 0.25*scipy.constants.g, inputTheta = 0.5, inputAngularVelocity = 0.1, updateMethod=1, updateMethodEnergy=1 ):
        self.mass = inputMass
        self.length = inputLength
        self.theta = inputTheta
        self.angularVelocity = inputAngularVelocity
        self.angularAcceleration = self.calculateAngularAcceleration()
        self.method = updateMethod
        self.methodEnergy = updateMethodEnergy
        self.kineticEnergy = self.calculatekineticEnergy()
        self.potentialEnergy = self.calculatepotentialEnergy()
        #self.time= inputTime
        #self.damping=inputDamping
        #self.period = inputPeriod
        #self.fd=inputFd
        
        
        #this will print out the numerical values 
    def __repr__(self):
        return 'Pendulum: m = {0} kg, l = {1} m, theta = {2}, ang. velocity = {3}, ang. acceleration = {4}, kinetic energy {5}, potential energy {6}' .format(self.mass,  self.length, self.theta, self.angularVelocity, self.angularAcceleration, self.kineticEnergy, self.potentialEnergy)
#
    def calculateAngularAcceleration(self, inputTime = 0.0, inputPeriod = math.pi/2.0,  inputFd=5.0, inputDamping=0.1 ): 
        self.time= inputTime
        self.period = inputPeriod
        self.fd=inputFd
        self.damping=inputDamping
        if (abs(self.time%self.period) < self.period/2.0):
            self.fd=self.fd
        else:
            self.fd=-self.fd
        self.angularAcceleration = (-1*scipy.constants.g/self.length)*math.sin(self.theta) - self.damping*self.angularVelocity #  +self.fd
        # or can do this 
        # self.angularAcceleration = value
        return self.angularAcceleration


    def update(self, delta_t):
        
       self.angularAcceleration = self.calculateAngularAcceleration()
       if self.method == 1:
            self.EulerCromer(delta_t)


    def EulerCromer(self, delta_t ):
        
        self.angularVelocity += self.angularAcceleration*delta_t
        self.theta += self.angularVelocity*delta_t


    def calculatekineticEnergy(self):
        
        
        self.kineticEnergy=1/2*self.mass*(self.length*self.angularVelocity)**2
        
        return self.kineticEnergy
    
    def updatekineticEnergy(self, delta_t):        
        self.kineticEnergy = self.calculatekineticEnergy()
        if self.methodEnergy == 1:
             self.EulerCromer(delta_t)

                 
    def calculatepotentialEnergy(self):
            
        self.potentialEnergy=self.mass*scipy.constants.g*self.length*(1-math.cos(self.theta))
        
        return self.potentialEnergy      
    
    def updatepotentialEnergy(self, delta_t):        
        self.potentialEnergy = self.calculatepotentialEnergy()
        if self.methodEnergy == 1:
             self.EulerCromer(delta_t)
    
