# Abstract
Pro 5.7
This article made a comparsion between Jacobi method and SOR algorithm to solve partial differential equation. Also, it compared the number of iterations and figured out the relation between the number of iterations and the number of grid elements.
# Background
## Laplace's equation
We get![G](http://latex.codecogs.com/png.latex?V(i,j,k)=\\frac{1}{6}[V(i+1,j,k)+V(i-1,j,k)+V(i,j+1,k)+V(i,j-1,k)+V(i,j,k+1)+V(i,j,k-1)])
## For the 2-dimensional case
![G](http://latex.codecogs.com/png.latex?V(i,j,k)=\\frac{1}{4}[V(i+1,j,k)+V(i-1,j,k)+V(i,j+1,k)+V(i,j-1,k)])
## Jacobi Method
![G](http://latex.codecogs.com/png.latex?V(i,j)_[new]=\\frac{1}{4}[V_[old](i+1,j)+V_[old](i-1,j)+V_[old](i,j+1)+V_[old](i,j-1)])
## Gauss-Sideal Method
![G](http://latex.codecogs.com/png.latex?V(i,j)_[new]=\\frac{1}{4}[V_[old](i+1,j)+V_[new](i-1,j)+V_[old](i,j+1)+V_[new](i,j-1)])
## So
![G](http://latex.codecogs.com/png.latex?V(i,j)=\\frac{1}{4}[V(i+1,j)+V(i-1,j)+V(i,j+1)+V(i,j-1)])
## And
![G](http://latex.codecogs.com/png.latex?\\alpha=2(1+\\pi/L))
# Code
## For Jacobi method:
    import matplotlib.pyplot as pl
    import numpy as np
    from mpl_toolkits.mplot3d import Axes3D
    from matplotlib import cm
    from matplotlib.ticker import LinearLocator, FormatStrFormatter
    class Jacobi:
    def __init__(self):
        pass
    def initialization(self,initial_file):
        itxt= open(initial_file)
        self.lattice_in = []
        self.lattice_out = []
        for lines in itxt.readlines():
            lines=lines.replace("\n","").split(",")
            #print (lines[0].split(" ")
            self.lattice_in.append(lines[0].split(" "))
            self.lattice_out.append(lines[0].split(" "))
        itxt.close()
        #print (self.lattice_in)
        #print (self.initial_lattice)
        for i in range(len(self.lattice_in)):
            for j in range(len(self.lattice_in[i])):
                self.lattice_in[i][j] = float(self.lattice_in[i][j])
                self.lattice_out[i][j] = float(self.lattice_out[i][j])
        return 0
    def evolution(self):
        delta = 10
        self.N = 0
        delta_record = []
        while (delta > 0.00001*len(self.lattice_in)*len(self.lattice_in[0])):
            delta = 0
            for i in range(1,len(self.lattice_in) - 1):
                for j in range(1,len(self.lattice_in[i]) - 1):
                    if (self.lattice_in[i][j] != 1 and self.lattice_in[i][j] != -1):
                        self.lattice_out[i][j] = 0.25*(self.lattice_in[i - 1][j] + self.lattice_in[i + 1][j] + self.lattice_in[i][j - 1] + self.lattice_in[i][j + 1])
                        delta+= abs(self.lattice_out[i][j] - self.lattice_in[i][j])
            delta_record.append(delta)
            for i in range(len(self.lattice_in)):
                for j in range(len(self.lattice_in[i])):
                    self.lattice_in[i][j] = self.lattice_out[i][j]
            self.N+= 1
        pl.plot(delta_record,label = 'jacobi method')
        print (self.N)
        print (len(self.lattice_in))
        return 0
    def electric_field(self):
        self.ex = np.zeros([len(self.lattice_in),len(self.lattice_in)])
        self.ey = np.zeros([len(self.lattice_in),len(self.lattice_in)])
        deltax = 0.3333
        deltay = 0.3333
        for i in range(1,len(self.lattice_in) - 1):
            for j in range(1,len(self.lattice_in[0]) - 1):
                self.ey[i][j] = (-(self.lattice_in[i + 1][j] - self.lattice_in[i - 1][j])/(2*deltax))
                self.ex[i][j] = (-(self.lattice_in[i][j + 1] - self.lattice_in[i][j - 1])/(2*deltay))
        return 0
    def plot_field(self):
        self.electric_field()
        X, Y = np.meshgrid(np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)), np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)))
        #pl.figure(figsize = (8,8))
        pl.quiver(X, Y, self.ex, self.ey, pivot = 'mid', units='width')
        pl.title('Electric field between two metal plates')
        #pl.show()
        return 0
    def plot_contour(self): 
        X, Y = np.meshgrid(np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)), np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)))
        #pl.figure(figsize = (8,8))
        CS = pl.contour(X, Y, self.lattice_in, 13)
        pl.clabel(CS, inline=1, fontsize=10)
        pl.title('Equipotential lines')
        #pl.show()
        return 0
    def plot_surface(self):
        fig = pl.figure()
        ax = fig.gca(projection='3d')
        X, Y = np.meshgrid(np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)), np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)))
        surf = ax.plot_surface(X, Y, self.lattice_in, rstride=1, cstride=1,cmap = cm.coolwarm_r,
                       linewidth=0, antialiased=False)
        ax.set_zlim(-1.01, 1.01)
        ax.zaxis.set_major_locator(LinearLocator(10))
        ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))
        fig.colorbar(surf, shrink=0.5, aspect=5)
    #A = Jacobi()
    A = SOR()
    A.initialization('initial_capacitor_26.txt')
    A.evolution()
    #pl.figure(figsize = (8,8))
    pl.figure(figsize = (16,8))
    pl.subplot(121)
    A.plot_contour()
    pl.subplot(122)
    A.plot_field()
    #A.plot_surface()
    pl.savefig('chapter5_5.7_26.png', dpi = 256)
pl.show()
## For SOR Method:
    class SOR(jacobi):
    def evolution(self):
        delta = 10
        self.N = 0
        delta_record = []
        alpha = 2/(1+np.pi/len(self.lattice_in))
        while (delta > 0.00001*len(self.lattice_in)*len(self.lattice_in[0])):
            delta = 0
            for i in range(1,len(self.lattice_in) - 1):
                for j in range(1,len(self.lattice_in[i]) - 1):
                    if (self.lattice_in[i][j] != 1 and self.lattice_in[i][j] != -1):
                        self.lattice_out[i][j] = 0.25*(self.lattice_in[i - 1][j] + self.lattice_in[i + 1][j] + self.lattice_in[i][j - 1] + self.lattice_in[i][j + 1])
                        delta+= abs(self.lattice_out[i][j] - self.lattice_in[i][j])
                        self.lattice_in[i][j] = alpha * (self.lattice_out[i][j] - self.lattice_in[i][j]) + self.lattice_in[i][j]
            #print (self.lattice_out)
            delta_record.append(delta)
            self.N+= 1
        pl.plot(delta_record, label ='SOR algorithm')
        print (self.N)
        print (len(self.lattice_in))
        #print (self.lattice_in)
        return 0    
    class SOR_2(jacobi):
    def evolution(self,alpha):
        delta = 10
        self.N = 0
        delta_record = []
        #alpha = 2/(1+np.pi/len(self.lattice_in))
        while (delta > 0.00001*len(self.lattice_in)*len(self.lattice_in[0])):
            delta = 0
            for i in range(1,len(self.lattice_in) - 1):
                for j in range(1,len(self.lattice_in[i]) - 1):
                    if (self.lattice_in[i][j] != 1 and self.lattice_in[i][j] != -1):
                        self.lattice_out[i][j] = 0.25*(self.lattice_in[i - 1][j] + self.lattice_in[i + 1][j] + self.lattice_in[i][j - 1] + self.lattice_in[i][j + 1])
                        delta+= abs(self.lattice_out[i][j] - self.lattice_in[i][j])
                        self.lattice_in[i][j] = alpha * (self.lattice_out[i][j] - self.lattice_in[i][j]) + self.lattice_in[i][j]
            #print (self.lattice_out)
            delta_record.append(delta)
            self.N+= 1
        #pl.plot(delta_record, label ='SOR algorithm')
        print (self.N)
        #print (len(self.lattice_in))
        #print (self.lattice_in)
        return 0  
# Conclusion
![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/z1.jpg)
![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/z2.png)
![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/z3.png)
![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/z4.png)
![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/z5.png)
In the different situations, we can get the same results as the Electromagnetics, so that the method of these two canbe used to solve the problems. Meanwhile, we can easily get the conclusion that the method SOR can be less number of iterations to the Jacobi method.
# Thanks
Baidu
