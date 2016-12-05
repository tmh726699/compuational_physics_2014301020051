(ps:上周作业上星期二才写完，如果助教没看到，麻烦您去看一眼，谢谢啦）

# Abstract
  There is one case of chaos in our solar system that is accessible to a fairly simple stimulation, that is the Hyperion. We will figure out 4.19 and 4.20 in follow discussion.
# Background
## The gravitational force on m1 is:
![G](http://latex.codecogs.com/png.latex?F_{1}=\\frac{GMm_{1}}{r_{1}^3}(x_{1}i+y_{1}j))
## The torque on m1 is
![T](http://latex.codecogs.com/png.latex?\\tau_{1}=[(x_{1}-x_{c})i+(y_{1}-y_{c})j]*F_{1})
## So we get
![G](http://latex.codecogs.com/png.latex?\\frac{d\\omega}{dt}=\\frac{\\tau_{1}+\\tau_{2}}{I})
## And ![G](http://latex.codecogs.com/png.latex?I=m_{1}|r_{1}|^2+m_{2}|r_{2}|^2),so
![G](http://latex.codecogs.com/png.latex?\\frac{d\\omega}{dt}=-\\frac{3GM}{r_{c}^5}(x_{c}sin\\theta-y_{c}cos\\theta)(x_{c}cos\\theta+y_{c}sin\\theta))
# code
    import math
    import matplotlib.pyplot as plt
    GM=4*(math.pi**2)
    class precession:
    def __init__(self,e=0.5,time=9,dt=0.0001,vcoefficient=1,beta=2,alpha=0,intial_w=0,intial_sita=0):
        self.intial_sita=intial_sita
        self.alpha=alpha
        self.beta=beta
        self.vcoefficient=vcoefficient
        self.e=e
        self.a=1/(1+e)
        self.x0=self.a*(1+e)
        self.y0=0
        self.vx0=0
        self.vy0=self.vcoefficient*math.sqrt((GM*(1-e))/(self.a*(1+e)))
        self.X=[self.x0]
        self.Y=[self.y0]
        self.Vx=[self.vx0]
        self.Vy=[self.vy0]
        self.w=[intial_w]
        self.sita=[intial_sita]
        self.T=[0]
        self.dt=dt
        self.time=time
        self.ThetaPrecession=[]
        self.TimePrecession=[]
    def calculate_reset(self):
        while self.T[-1]<self.time:
            r=math.sqrt(self.X[-1]**2+self.Y[-1]**2)
            newVx=self.Vx[-1]-(GM*(1+self.alpha/r**2)*self.X[-1]/r**(1+self.beta))*self.dt
            newX=self.X[-1]+newVx*self.dt
            newVy=self.Vy[-1]-(GM*(1+self.alpha/r**2)*self.Y[-1]/r**(1+self.beta))*self.dt
            newY=self.Y[-1]+newVy*self.dt
            newW=self.w[-1]-3*GM/(r**5)*(self.X[-1]*math.sin(self.sita[-1])-self.Y[-1]*math.cos(self.sita[-1]))*(self.X[-1]*math.cos(self.sita[-1])+self.Y[-1]*math.sin(self.sita[-1]))*self.dt
            newSita=self.dt*self.w[-1]+self.sita[-1]         
            self.Vx.append(newVx)
            self.Vy.append(newVy)
            self.X.append(newX)
            self.Y.append(newY)
            self.w.append(newW)
            self.sita.append(newSita)
            self.T.append(self.T[-1]+self.dt)            
            while self.sita[-1]>=1*math.pi:
                self.sita[-1]=self.sita[-1]-2*math.pi
            while  self.sita[-1]<=-1*math.pi:
                self.sita[-1]=self.sita[-1]+2*math.pi                        
    def calculate_nonreset(self):
        while self.T[-1]<self.time:
            r=math.sqrt(self.X[-1]**2+self.Y[-1]**2)
            newVx=self.Vx[-1]-(GM*(1+self.alpha/r**2)*self.X[-1]/r**(1+self.beta))*self.dt
            newX=self.X[-1]+newVx*self.dt
            newVy=self.Vy[-1]-(GM*(1+self.alpha/r**2)*self.Y[-1]/r**(1+self.beta))*self.dt
            newY=self.Y[-1]+newVy*self.dt
            newW=self.w[-1]-3*GM/(r**5)*(self.X[-1]*math.sin(self.sita[-1])-self.Y[-1]*math.cos(self.sita[-1]))*(self.X[-1]*math.cos(self.sita[-1])+self.Y[-1]*math.sin(self.sita[-1]))*self.dt
            newSita=self.dt*self.w[-1]+self.sita[-1]
            self.Vx.append(newVx)
            self.Vy.append(newVy)
            self.X.append(newX)
            self.Y.append(newY)
            self.w.append(newW)
            self.sita.append(newSita)
            self.T.append(self.T[-1]+self.dt)
    def plot(self,slogan=''):
        plt.scatter(self.sita,self.w,label=slogan,s=0.1)
        plt.legend(loc='upper right',frameon=False,fontsize='small')
        plt.grid(True)
        #plt.title('Phase Plot(Elleptical Orbit) e=0.206 $\\theta$ versus $w$')
        #plt.title('The Hyperion(Circular orbit) $w$ versus time')
        plt.title('Hyperion(Elliptical orbit) $\\omega$ versus $\\theta$')
        plt.title('The Hyperion(Elleptical orbit) $w$ versus time')
        plt.ylabel('$\\omega(radians/yr)$')        
        plt.xlabel('$\\theta$')
        plt.xlabel('$\\theta(radians)$')
        plt.xlim(-0.6,0.6)
        plt.ylim(-0.6,0.6)
        plt.scatter(0,0)
    a=precession(intial_sita=0)
    a.calculate_nonreset()
    a.plot()
    b=precession(intial_sita=0.01)
    b.calculate_nonreset()
    c=[]
    for i in range (len(a.T)):
    c.append(abs(a.sita[i]-b.sita[i]))
    plt.semilogy(a.T,c)
    plt.title('Hyperion(Elliptical orbit) $\\Delta\\theta$ versus time')
    plt.ylabel('$\\Delta\\theta(radians)$')        
    plt.xlabel('$timr(yr)$')
    plt.xlim(0,6)
    plt.ylim(0,10)
# Conclusion
## dt=0.0001yr
![](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/h%3D0.0001yr.png)
## ![G](http://latex.codecogs.com/png.latex?\\theta(1)=0,\\theta(2)=0.01)
![](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/theta1 2.png)
![](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/p h.png)
## dt=0.0001yr,e=0.365
![](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/e.png)
## e=0.5
![](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/e0.5.png)
## ![G](http://latex.codecogs.com/png.latex?\\theta(1)=0,\\theta(2)=0.01,e=0.365)
![](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/theta e.png)
# Thanks
peiyu
