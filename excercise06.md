# Abstract
  “Precise Strike System”
# Background
  The error of firing angle is ![公式名](http://latex.codecogs.com/png.latex?\\pm2^{o}),the error of speed is 5%,the eroor of
  head resistance is 10%.The mean square of missile impact point to target smaller is the superior.
# Text
## Code
    #coding:utf-8
    import pylab as pl    import math    import random
    class cannon_shell:
    def __init__(self,initial_velocity=0.7,g=0.0098,range=0,height=0,target_range=10,target_height=10,time_step=0.005,\
                 da=0.00001*math.pi,\
                 wind_speed=0.01,accuracy=0.01):
        self.v=initial_velocity        self.Vw=wind_speed
        self.a=math.atan((target_height-height)/(target_range-range))
        self.g=g
        self.dt=time_step
        self.da=da        self.x=[range]
        self.y=[height]        self.xt=target_range
        self.yt=target_height
        self.b_m=0.004
        self.ture_x=[]
        self.ture_y=[]        self.dxdy=accuracy

    def pre_run_test(self):
        _a= 0.001*math.pi        da= 0.001*math.pi
        dxdy=1.0
        aa=[]
        t=7
        a_max=0.5 * math.pi
        while(t>0):
            can2=0 #break
            while (_a < a_max):                can = 0  # judge
                vx = math.cos(_a) * self.v
                vy = math.sin(_a) * self.v
                xi = [0]
                yi = [0]
                while (yi[-1] >= 0):
                    xi.append(xi[-1] + self.dt * vx)
                    yi.append(yi[-1] + self.dt * vy)
                    v = ((vx + self.Vw)**2+vy**2)**0.5
                    vx = vx \
                         - math.exp(-yi[-1] / 10) * self.b_m * v * vx * self.dt
                    vy = vy \
                         - self.g * self.dt \
                         - math.exp(-yi[-1] / 10) * self.b_m * v * vy * self.dt
                    if (abs(xi[-1] - self.xt) < dxdy and abs(yi[-1] - self.yt) < dxdy):  # judge
                        can = 1
                        break
                if can: #find familiar angle
                    aa.append(_a)
                    can2=2
                elif can==0 and can2>1: break #break unnecessary loop
                _a = _a + da
            if aa==[]:
                break
            _a=min(aa)-da
            a_max=max(aa)+da
            da=da/5
            dxdy=dxdy/4
            _aa =aa
            aa=[]
            t-=1
        self.a= 0.5*max(_aa)+0.5*min(_aa)
        self.dxdy=dxdy*4

    def run(self):   #mapping
        _a = self.a
        vx = math.cos(_a) * self.v
        vy = math.sin(_a) * self.v
        xi = [0]
        yi = [0]
        t=0
        while (yi[-1] >= 0):
            t=t+self.dt
            xi.append(xi[-1] + self.dt * vx)
            yi.append(yi[-1] + self.dt * vy)
            v = (vx + self.Vw) / math.cos(_a)
            vx = vx \
                 - math.exp(-yi[-1] / 10) * self.b_m * v * vx * self.dt
            vy = vy \
                 - self.g * self.dt \
                 - math.exp(-yi[-1] / 10) * self.b_m * v * vy * self.dt
            if (abs(xi[-1] - self.xt) < self.dxdy and abs(yi[-1] - self.yt) < self.dxdy):  # judge
                print abs(xi[-1] - self.xt), abs(yi[-1] - self.yt)
                print abs(xi[-1]), yi[-1],t
                print ((xi[-1] - self.xt)**2+(yi[-1] - self.yt)**2)**0.5
        self.ture_x = xi
        self.ture_y = yi

    def show_result(self):
        x_y=self.ture_y+self.ture_x
        xy=max(x_y)+1
        pl.plot(self.xt,self.yt,'r*')
        pl.plot(self.ture_x,self.ture_y,color="green",linewidth=1)
        pl.xlabel('x ($m$)')
        pl.ylim(0, xy)
        pl.xlim(0, xy)
        pl.ylabel('y ($m$)')
        pl.show()
    a=cannon_shell()
    a.pre_run_test()
    a.run()
    a.show_result()
# Conclusion
## Figure
   ![1](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/%E6%88%AA%E5%9B%BE1.png)
   ![2]()
# Thanks
  thanks to ZouZhiyuan.
