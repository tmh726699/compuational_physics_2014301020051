# Abstract
  The billiard system can also be a chaotic system,I will consider one more chaotice model in this paper.
# Background
  The billiard ball will move without friction on a perfect billiard table.
## Velocity
  ![Velocity](http://latex.codecogs.com/png.latex?\\frac{dx}{dt}=v_{x}),
  ![Velocity](http://latex.codecogs.com/png.latex?\\frac{dy}{dt}=v_{y}).
## Collision
  ![Collision](http://latex.codecogs.com/png.latex?v_{f, ver}=-v_{i,ver}),
  ![Collision](http://latex.codecogs.com/png.latex?v_{f, par}=-v_{i,par}).
## The Lorenz Attractor
  ![The Lorenz Attractor](http://latex.codecogs.com/png.latex?\\frac{dx}{dt}=\\sigma(y-x)),
  ![The Lorenz Attractor](http://latex.codecogs.com/png.latex?\\frac{dy}{dt}=-xz+rx-y),
  ![The Lorenz Attractor](http://latex.codecogs.com/png.latex?\\frac{dz}{dt}=xy-bz).
# Code
## In the case of square boundary
    import matplotlib.pyplot as plt
    import numpy as np
    class billiard_rectangular:
    def __init__(self,x_0,y_0,vx_0,vy_0,N,dt):
        self.x_0 = x_0
        self.y_0 = y_0
        self.vx_0 = vx_0
        self.vy_0 = vy_0
        self.N = N
        self.dt = dt
    def motion_calculate(self):
        self.x = []
        self.y = []
        self.vx = []
        self.vy = []
        self.t = [0]
        self.x.append(self.x_0)
        self.y.append(self.y_0)
        self.vx.append(self.vx_0)
        self.vy.append(self.vy_0)
        for i in range(1,self.N):
            self.x.append(self.x[i - 1] + self.vx[i - 1]*self.dt)
            self.y.append(self.y[i - 1] + self.vy[i - 1]*self.dt)
            self.vx.append(self.vx[i - 1])
            self.vy.append(self.vy[i - 1])
            if (self.x[i] < -1.0):
                self.x[i],self.y[i] = self.correct('x>-1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i] = - self.vx[i]
            elif(self.x[i] > 1.0):
                self.x[i],self.y[i] = self.correct('x<1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i] = - self.vx[i]
            elif(self.y[i] < -1.0):
                self.x[i],self.y[i] = self.correct('y>-1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vy[i] = -self.vy[i]
            elif(self.y[i] > 1.0):
                self.x[i],self.y[i] = self.correct('y<1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vy[i] = -self.vy[i] 
            else:
                pass
            self.t.append(self.t[i - 1] + self.dt)
        return self.x, self.y
    def correct(self,condition,x,y,vx,vy):
        vx_c = vx/100.0
        vy_c = vy/100.0
        while eval(condition):
            x = x + vx_c*self.dt
            y = y + vy_c*self.dt
        return x-vx_c*self.dt,y-vy_c*self.dt
    def reflect(self):
        pass
    def plot(self):
        plt.figure(figsize = (8,8))
        plt.xlim(-1,1)
        plt.ylim(-1,1)
        plt.xlabel('x')
        plt.ylabel('y')
        plt.plot(self.x,self.y,'g')
        plt.show()
## In the case of elliptical boundary
    class billiard_ellipse(billiard_circle):   ### x^2/3+y^2/2 = 1
    def motion_calculate(self):
        self.x = []
        self.y = []
        self.vx = []
        self.vy = []
        self.t = [0]
        self.x.append(self.x_0)
        self.y.append(self.y_0)
        self.vx.append(self.vx_0)
        self.vy.append(self.vy_0)
        for i in range(1,self.N):
            self.x.append(self.x[i - 1] + self.vx[i - 1]*self.dt)
            self.y.append(self.y[i - 1] + self.vy[i - 1]*self.dt)
            self.vx.append(self.vx[i - 1])
            self.vy.append(self.vy[i - 1])
            if (self.x[i]**2/3+self.y[i]**2/2 > 1.0):
                self.x[i],self.y[i] = self.correct('x**2/3+y**2/2 < 1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i],self.vy[i] = self.reflect((2./3)*self.x[i],self.y[i],self.vx[i - 1], self.vy[i - 1])
            self.t.append(self.t[i - 1] + self.dt)
        return self.x, self.y       
    def plot(self):
        plt.figure(figsize = (8,6))
        plt.xlim(-2.2,2.2)
        plt.ylim(-2,2)
        plt.xlabel('x')
        plt.ylabel('y')
        self.plot_boundary()
        plt.plot(self.x,self.y,'b')
        plt.show()
    def plot_boundary(self):
        theta = 0
        x = []
        y = []
        while theta < 2*np.pi:
            x.append(np.sqrt(3)*np.cos(theta))
            y.append(np.sqrt(2)*np.sin(theta))
            theta+= 0.01
        plt.title(r'Elliptical stadium  $\frac{x^2}{4}+\frac{y^2}{3} = 3$')
        plt.plot(x,y)
## In the case of square with an inner circle boundary, and the circle is in the center
    class billiard_innercircle(billiard_circle):
    def motion_calculate(self):
        self.x = []
        self.y = []
        self.vx = []
        self.vy = []
        self.t = [0]
        self.x.append(self.x_0)
        self.y.append(self.y_0)
        self.vx.append(self.vx_0)
        self.vy.append(self.vy_0)
        for i in range(1,self.N):
            self.x.append(self.x[i - 1] + self.vx[i - 1]*self.dt)
            self.y.append(self.y[i - 1] + self.vy[i - 1]*self.dt)
            self.vx.append(self.vx[i - 1])
            self.vy.append(self.vy[i - 1])
            if (self.x[i] < -1.0):
                self.x[i],self.y[i] = self.correct('x>-1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i] = - self.vx[i]
            elif(self.x[i] > 1.0):
                self.x[i],self.y[i] = self.correct('x<1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i] = - self.vx[i]
            elif(self.y[i] < -1.0):
                self.x[i],self.y[i] = self.correct('y>-1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vy[i] = -self.vy[i]
            elif(self.y[i] > 1.0):
                self.x[i],self.y[i] = self.correct('y<1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vy[i] = -self.vy[i] 
            elif(self.x[i]**2+self.y[i]**2<0.01):
                self.x[i],self.y[i] = self.correct('x**2+y**2 > 0.01',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i],self.vy[i] = self.reflect(self.x[i],self.y[i],self.vx[i - 1], self.vy[i - 1])
            self.t.append(self.t[i - 1] + self.dt)
        return self.x, self.y    
    def plot_boundary(self):
        theta = 0
        x = []
        y = []
        while theta < 2*np.pi:
            x.append(0.1*np.cos(theta))
            y.append(0.1*np.sin(theta))
            theta+= 0.01
        plt.plot(x,y)
## In the case of square with an inner circle boundary and the circle is slightly off-center
    class billiard_innercircle2(billiard_circle):
    def motion_calculate(self):
        self.x = []
        self.y = []
        self.vx = []
        self.vy = []
        self.t = [0]
        self.x.append(self.x_0)
        self.y.append(self.y_0)
        self.vx.append(self.vx_0)
        self.vy.append(self.vy_0)
        for i in range(1,self.N):
            self.x.append(self.x[i - 1] + self.vx[i - 1]*self.dt)
            self.y.append(self.y[i - 1] + self.vy[i - 1]*self.dt)
            self.vx.append(self.vx[i - 1])
            self.vy.append(self.vy[i - 1])
            if (self.x[i] < -1.0):
                self.x[i],self.y[i] = self.correct('x>-1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i] = - self.vx[i]
            elif(self.x[i] > 1.0):
                self.x[i],self.y[i] = self.correct('x<1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i] = - self.vx[i]
            elif(self.y[i] < -1.0):
                self.x[i],self.y[i] = self.correct('y>-1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vy[i] = -self.vy[i]
            elif(self.y[i] > 1.0):
                self.x[i],self.y[i] = self.correct('y<1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vy[i] = -self.vy[i] 
            elif((self.x[i] - 0.05)**2+self.y[i]**2<0.01):
                self.x[i],self.y[i] = self.correct('(x - 0.05)**2+y**2 > 0.01',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i],self.vy[i] = self.reflect(self.x[i]-0.05,self.y[i],self.vx[i - 1], self.vy[i - 1])
            self.t.append(self.t[i - 1] + self.dt)
        return self.x, self.y    
    def plot_boundary(self):
        theta = 0
        x = []
        y = []
        plt.title(r'$(x-0.5)^2+y^2 = 0.01$')
        while theta < 2*np.pi:
            x.append(0.1*np.cos(theta)+0.5)
            y.append(0.1*np.sin(theta))
            theta+= 0.01
        plt.plot(x,y)
## In the case of circular boundary
    class billiard_circle(billiard_rectangular):
    def motion_calculate(self):
        self.x = []
        self.y = []
        self.vx = []
        self.vy = []
        self.t = [0]
        self.x.append(self.x_0)
        self.y.append(self.y_0)
        self.vx.append(self.vx_0)
        self.vy.append(self.vy_0)
        for i in range(1,self.N):
            self.x.append(self.x[i - 1] + self.vx[i - 1]*self.dt)
            self.y.append(self.y[i - 1] + self.vy[i - 1]*self.dt)
            self.vx.append(self.vx[i - 1])
            self.vy.append(self.vy[i - 1])
            if (np.sqrt( self.x[i]**2+self.y[i]**2 ) > 1.0):
                self.x[i],self.y[i] = self.correct('np.sqrt(x**2+y**2) < 1.0',self.x[i - 1], self.y[i - 1], self.vx[i - 1], self.vy[i - 1])
                self.vx[i],self.vy[i] = self.reflect(self.x[i],self.y[i],self.vx[i - 1], self.vy[i - 1])
            self.t.append(self.t[i - 1] + self.dt)
        return self.x, self.y
    def reflect(self,x,y,vx,vy):
        module = np.sqrt(x**2+y**2)  ### normalization
        x = x/module
        y = y/module
        v = np.sqrt(vx**2+vy**2)
        cos1 = (vx*x+vy*y)/v
        cos2 = (vx*y-vy*x)/v
        vt = -v*cos1
        vc = v*cos2 
        vx_n = vt*x+vc*y
        vy_n = vt*y-vc*x
        return vx_n,vy_n
    def plot(self):
        plt.figure(figsize = (8,8))
        plt.xlim(-1,1)
        plt.ylim(-1,1)
        plt.xlabel('x')
        plt.ylabel('y')
        self.plot_boundary()
        plt.plot(self.x,self.y,'b')
        plt.show()
    def phase_space_plot(self):
        plt.figure(figsize = (16,8))
        plt.subplot(121)
        plt.xlabel('x')
        plt.ylabel(r'$v_x$')
        plt.scatter(self.x,self.vx, s=1)
        plt.subplot(122)
        plt.xlabel('y')
        plt.ylabel(r'$v_x$')
        plt.scatter(self.y,self.vx, s=1)
        plt.show()    
    def phase_plot(self):
        plt.figure(figsize = (8,8))
        record_x = []
        record_vx = []
        for i in range(len(self.x)):
            if (abs(self.y[i] - 0)<0.001):
                record_vx.append(self.vx[i])
                record_x.append(self.x[i])
        plt.xlabel('x')
        plt.ylabel(r'$v_x$')
        plt.scatter(record_x,record_vx,s=1)
        plt.show()
    def plot_boundary(self):
        theta = 0
        x = []
        y = []
        while theta < 2*np.pi:
            x.append(np.cos(theta))
            y.append(np.sin(theta))
            theta+= 0.01
        plt.plot(x,y)
# Conclusion
## In the case of square boundary
  ![s]()
## In the case of elliptical boundary
## In the case of square with an inner circle boundary, and the circle is in the center
## In the case of square with an inner circle boundary and the circle is slightly off-center
## In the case of circular boundary
