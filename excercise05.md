
# 摘要
  运用欧拉方程计算炮弹的轨迹
# 背景
## 牛顿第二定律
   ![原方程](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/%E7%89%9B2.png)
##  处理使用
   ![变形为](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/%E5%A4%84%E7%90%86.png)
## 位移和速度关系
   ![关系式](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/%E4%BD%8D%E7%A7%BB%E5%92%8C%E9%80%9F%E5%BA%A6.png)
# 正文
## 代码
import pylab as pl
class cannon:
    def __init__(self, vel_of_x=50,vel_of_y=50,time_of_duration = 10, time_step = 0.05,value_of_x=0,value_of_y=0,g=10):
        self.vx = [vel_of_x]
        self.vy = [vel_of_y]
        self.x = [value_of_x]
        self.y = [value_of_y]
        self.t = [0]
        self.g = g
        self.dt = time_step
        self.time = time_of_duration
        self.nsteps = int(time_of_duration // time_step + 1)
    def calculate(self):
        for i in range(self.nsteps):
            tmpvx = self.vx[i]
            tmpvy = self.vy[i] - (self.g * self.dt)
            tmpx=self.x[i] + self.vx[i] * self.dt
            tmpy=self.y[i] + self.vy[i] * self.dt
            self.vx.append(tmpvx)
            self.vy.append(tmpvy)
            self.x.append(tmpx)
            self.y.append(tmpy)
            self.t.append(self.t[i] + self.dt)
    def show_results(self):
        pl.plot(self.x, self.y)
        pl.xlabel('x ($m$)')
        pl.ylabel('y ($m$)')
        pl.show()
a = cannon()
a.calculate()
a.show_results()
