# Abstact
  With the help of the Kepler's Laws,we investigate a few of the properities of this model solar system.And we use the Euler-Cromer method to solve the trobule of the precession of perihelion of Mercury. 
# Background
## Newton's law of the gravitation:
### ![G](http://latex.codecogs.com/png.latex?F_{g}=\\frac{GM_{S}M_{E}}{r^2})
## Newton's secnd law
### ![S](http://latex.codecogs.com/png.latex?\\frac{d^2x}{dt^2}=\\frac{F_{G,x}}{M_{E}})
### ![S](http://latex.codecogs.com/png.latex?\\frac{d^2y}{dt^2}=\\frac{F_{G,y}}{M_{E}})
### Thus ![S](http://latex.codecogs.com/png.latex?F_{G,x}=-\\frac{GM_{S}M_{E}}{r^2}cos\\theta=\\frac{GM_{S}M_{Ex}}{r^2})
### We can get
### ![S](http://latex.codecogs.com/png.latex?\\frac{dV_{x}}{dt}=\\frac{-GM_{sy}}{r^3})
### ![S](http://latex.codecogs.com/png.latex?\\frac{dy}{dt}=V_{y})
### We obtain ![S](http://latex.codecogs.com/png.latex?\\frac{d^2y}{dt^2}=\\frac{F_{G,y}}{M_{E}})
# Code
    import math
    import matplotlib.pyplot as plt
    GM=4*(math.pi**2)
    perihelion=0.39*(1-0.206)
    class Precession:
        def __init__(self,e=0.206,time=1,dt=0.0001,vcoefficient=1.1,beta=2.1,alpha=0.01):
        self.alpha=alpha
        self.beta=beta
        self.vcoefficient=vcoefficient
        self.e=e
        self.a=perihelion/(1-e)
        self.x0=self.a*(1+e)
        #self.x0=1
        self.y0=0
        self.vx0=0
        self.vy0=self.vcoefficient*math.sqrt((GM*(1-self.e))/(self.a*(1+self.e)))
        #self.vy0=2*math.pi
        self.X=[self.x0]
        self.Y=[self.y0]
        self.Vx=[self.vx0]
        self.Vy=[self.vy0]
        self.T=[0]
        self.dt=dt
        self.time=time
        self.ThetaPrecession=[]
        self.TimePrecession=[]
    def run(self):
        while self.T[-1]<self.time:
            r=math.sqrt(self.X[-1]**2+self.Y[-1]**2)
            newVx=self.Vx[-1]-(GM*(1+self.alpha/r**2)*self.X[-1]/r**(1+self.beta))*self.dt
            newX=self.X[-1]+newVx*self.dt
            newVy=self.Vy[-1]-(GM*(1+self.alpha/r**2)*self.Y[-1]/r**(1+self.beta))*self.dt
            newY=self.Y[-1]+newVy*self.dt
            if abs(newX*newVx+newY*newVy)<0.0014 and r<self.a:
                theta=math.acos(self.X[-1]/r)*180/math.pi
                if (self.Y[-1]/r)<0:
                    theta=360-theta
                theta=abs(theta-180)
                self.ThetaPrecession.append(theta)
                self.TimePrecession.append(self.T[-1])
            self.Vx.append(newVx)
            self.Vy.append(newVy)
            self.X.append(newX)
            self.Y.append(newY)
            self.T.append(self.T[-1]+self.dt)
              def orientation(self):
        plt.plot(self.TimePrecession,self.ThetaPrecession)
        plt.scatter(self.TimePrecession,self.ThetaPrecession)
        plt.legend(loc='upper right',frameon=False,fontsize='xx-small')
        plt.grid(True)
        plt.title('Orbit orientation versus time')
        plt.xlabel('time(yr)')
        plt.ylabel(r'$\theta$(degrees)')
        #print (self.ThetaPrecession)
        #print (self.TimePrecession)
A = Precession()
A.run()
A.show_results()
# Conclusion
## First, we can chose that ![G](http://latex.codecogs.com/png.latex?\\alpha=0.0008,\\beta=2,e=0.206)
  ![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/k1.png)
## Then, we chose that ![G](http://latex.codecogs.com/png.latex?\\alpha=0.0008,\\beta=2,e=0.3、0.4、0.5)
  ![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/k2.png)
## And we chose that ![G](http://latex.codecogs.com/png.latex?\\alpha=0.01,\\beta=2,e=0.206、0.3、0.4、0.5)
  ![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/k3.png)
## When we chose that ![G](http://latex.codecogs.com/png.latex?\\alpha=0.01,\\beta=2.01)
  ![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/k4.png)
## Finally, we chose that ![G](http://latex.codecogs.com/png.latex?V=1.1V_{0},\\alpha=0.01)
  ![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/k5.png)
# Thanks
  Baidu
























