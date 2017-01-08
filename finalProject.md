# Final Project:Random Systems
  物基一班 滕明航 学号2014301020051
  
# Abstract
      We consider a class of systems in which randomness plays a central role.These are called random or stochastic 
    systems.Randomness can then arise in several different ways.
# Background
      A statistical description is often extremely useful,and indeed,most appropriate in these problems.Let us consider 
    what happens if we start with a cup of black coffee and then delicately place a drop of cream at the center.Our 
    goal is to construct a useful theoretical description of the way the cream mixes with the coffee.Our point is that
    such a complete computational solution of the problem would give far more information than we want or need to 
    nuder stand the mixing process.This discussion of the cream-in-your-coffee problem was intended to introduce you
    to a situation in which a statistical approch provides precisely the type of information we are after.
# The main body
## Random walk
### 1D
      The simplest situation involves a walker that is able to take steps of length unity along a line.This one dimension 
      random walk is illustrated schematimacally in figure.The walker begins at the origin, x=0, and the first step is 
      chosen at random to be either to the left or right, each with the probability 1/2. And the next step was then 
      chosen, and again the probabilities for stepping lift or right are both 1/2. In this example the step went left,
      so x2=0.This process can be repeated,and the position as a molecule in solution,the time between steps is 
      approximately a constant,so the step number is roughly proportional to time.We will,therefore,often refer to the
      walker's position as a function of time.A routine that implements a random walk in one dimension is illustrated 
      below.Here we gennerate a random number in the range between 0 and 1 and compare its value to 1/2.If it is less
      than 1/2,otherwise it steps to the left.This process is then repeated to generate xn as a function of n.Any 
      suitable pseudo-random number generator can be used(see Appendix F),but here we use a function we choose to call rnd.
#### Code
    def __init__(self,x=0,y=0,z=0):
        self.x=[x]
        self.y=[y]
        self.z=[z]
        self.temx=0
        self.temy=0
        self.temz=0
        self.direct=0
        self.n=[0]
        self.dn=0
        self.squx=[0]*100
        self.squy=[0]*100
        self.squz=[0]*100
        self.r=[0]*100
    def run(self):
        for n in range(1000):
            s=random.randint(0,150)
            if s<=25:
                self.temy=self.temy+1
            elif s<=50:
                self.temy=self.temy-1
            elif s<=75:
                self.temx=self.temx-1
            elif s<=100:
                self.temx=self.temx+1
            elif s<=125:
                self.temz=self.temz+1
            else:
                self.temz=self.temz-1
            self.dn=self.dn+1
            self.x.append(self.temx)
            self.y.append(self.temy)
            self.z.append(self.temz)
            self.n.append(self.dn)
#### Result
##### n=100:
   ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n100.png)
##### n=1000:
   ![1000](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n1000.png)
##### n=10000:
   ![10000](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n10000.png)
##### n=100000:
   ![100000](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n100000.png)
##### n=1000All:
   ![1111111](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n1111111.png)
##### n=1000the picture x^2 versus t.
   ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n1000x2.png)
#### Choose the step length to right is as 1.5 times and 4 times as to left.
##### 1.5times:
   ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/p1.5.png)
##### 4times:
   ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/p4.png)
##### Together:
   ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/p t.png)

### 2D
    We have two directions to walk. The walker is able to take steps of length unity along two lines. So, 
    we can solve the problem in a lattice.We can le the walker walk in ang way, thus it's the real 2D 
    random walk, but not the random in a 2D lattice random walk. 
#### Code
    for n in range(100):
            s=random.randint(0,628)
            s=s/100
            self.dn=self.dn+1
            self.temx=self.temx+1*math.cos(s)
            self.temy=self.temy+1*math.sin(s)
            self.x.append(self.temx)
            self.y.append(self.temy)
            self.n.append(self.dn)
#### Result
##### n=100:
  ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n100%EF%BC%9B.png)
##### n=1000:
  ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n1000%EF%BC%9B.png)
##### n=10000:
  ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n10000%EF%BC%9B.png)
##### n=1000all：
  ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n1111111%EF%BC%9B.png)
##### n=1000,the picture p^2 versus t:
  ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/n1000p2%EF%BC%9B.png)
##### choose the step length to x is as 3 times as to y:
  ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/x=3y.png)
##### the step length of x is 1, 3 and 5 times of the step length of y:
  ![100](https://github.com/tmh726699/compuational_physics_2014301020051/blob/master/x=1 3 5y.png)
# Conclusion
        For random walk, we can not to clone two identical random walk pattern, which means this process is
      absolutely random. Also it's easy to know that for the average rn is zeor in all 1,2,3 D and the for
      the average r^2 is propotional to step number in all 1,2,3 D. Also for the diffusion constant D, when
      we change the step length in different direction, the D also be a constant in all 1,2,3 D, but if we 
      change the probability in different direction, D will not be a constant.
        In fact, the conservation of any non - regular Walker corresponds to a law of diffusion. Although any 
      single step does not obey the law of diffusion, it is possible to accurately predict the irregular walk
      if you wait long enough. Through the program to simulate the random walk behavior of a large number of 
      particles, we can intuitively understand the characteristics of the diffusion phenomenon, and realize 
      that the diffusion phenomenon has the randomness of the local motion and the certainty of the macroscopic
      quantity. Also, we can use random work to study the problem in daily life.
# Thanks
      Pólya's Random Walk Constants, Mathworld.wolfram.com. Retrieved 2016.
      Nicholas J.Giordano;Hisao Nakanishi,Computational Physics,second edition,Pearson Education.2007.
      Baidu.
