```python
#problem3.12代码

import matplotlib.pyplot as pl
import math
class Poincare_section:
    """
    To make the plot corresponding to a maximum of the drive force,
    and at times pi/4 out-of-phase with this force.
    Programming with ouler_cromer_caculate method.
    """
    def __init__(self, F_D, total_time):
        self.theta = [0.2]
        self.omiga = [0]
        self.g = 9.8
        self.dt = 0.04
        self.time = total_time
        self.t = [0]
        self.pendulum_length = 9.8
        self.q = 0.5
        self.F_D = F_D
        self.OMIGA_D = 2.0 / 3
        self.g_l = self.g / self.pendulum_length
        self.tau = 0.04
        self.t1 = [0]

    def run(self):
        for i in range(1,self.time):
            self.theta.append(self.theta[i - 1] + self.omiga[i - 1] * self.dt)
            self.t.append(self.t[i - 1] + self.dt)
            self.omiga.append(self.omiga[i - 1] - (self.g_l * \
                math.sin(self.theta[i]) + self.q * \
                self.omiga[i - 1] - self.F_D * math.sin(self.OMIGA_D *\
                self.t[i])) * self.dt)
            if self.theta[-1] >= math.pi:
                self.theta[-1] = -2 * math.pi + self.theta[-1]
     
            elif self.theta[-1] <= -math.pi:
                self.theta[-1] = self.theta[-1] + 2 * math.pi

    def show_results1(self):
        for i in range(len(self.t)):
            pl.scatter([self.theta[i], ], [ self.omiga[i],], 1, color ='blue')
            
        pl.title('Trajectory of $w$ versus $\ theta $')
        pl.ylabel('$w$ ($radians/s$)')
        pl.xlabel('$\ theta $ ($radians/s$)')
        pl.text(-0.5, 0.7, '$w$ versus $\ theta $' +\
                ' ' + '$F_D = $' + str(self.F_D))
        pl.show()

    def show_results2(self):
        for i in range(len(self.t)):
            
 #           delta = (self.OMIGA_D * self.t[i]) / (2 * math.pi) 
 #           delta = (self.OMIGA_D * self.t[i]) / (2 * math.pi) - 1.0 / 2.0
 #           delta = (self.OMIGA_D * self.t[i]) / (2 * math.pi) - 1.0 / 8.0
             delta = (self.OMIGA_D * self.t[i]) / (2 * math.pi) - 1.0 / 4.0
            if abs(delta - round(delta)) <= 0.001:

                self.t1.append(self.t[i])
                pl.scatter([self.theta[i], ], [self.omiga[i], ], 1, color ='red')
       
        
        pl.title('Phase space of $w$ versus $\ theta $')
        pl.ylabel('$w$ ($radians/s$)')
        pl.xlabel("$\ theta $ ($radians/s$)")
        pl.text(0, 1.2, '$w$ versus $\ theta $  ' +\
                ' ' + '$F_D = $' + str(self.F_D))
        pl.show()
        
a = Poincare_section(1.2, 1000000)
a.run()
a.show_results1()
a.show_results2()

a = Poincare_section(0.5, 1000000)
a.run()
a.show_results1()
a.show_results2()
```
