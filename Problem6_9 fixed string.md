```python
import pylab as pl
import math as m
import numpy as np

class string():
    
    def __init__(self, epcilong):
        self.dx = 0.01
        self.length = int(1 / self.dx)
        self.c = 300.0
        self.dt = self.dx / (4 * self.c)
        self.time = 3000
        self.r = self.c * self.dt / self.dx 
        self.epcilong = epcilong
        self.M = 1 / self.dx
        self.Mcr = self.epcilong * self.r**2 * self.M**2
        self.cM = 1 + 4 * self.epcilong * self.M**2
        k = 1000
        x0 = 0.5
        self.y0 = [[0 for i in range(self.length)] for n in range(self.time)]
        self.x = [0]
        self.t = [0]
        for i in range(self.length -1):
            self.x.append(self.x[i] + self.dx)
        for n in range(self.time - 1):
            self.t.append(self.t[n] + self.dt)
        print len(self.x)
        for n in range(self.time):
            for i in range(self.length):
                self.y0[n][i] = m.exp(-k * (self.x[i] - x0)**2)
            
        print len(self.y0[0])
        
    def update(self):
        self.y = self.y0   
        for n in range(self.time - 2):
            for i in range(1, self.length - 2):
                self.y[n+2][i] = 2 * (1 - self.r**2 - 3 * self.Mcr) * self.y[n+1][i]\
                              - self.y[n][i] + self.r**2 * self.cM * (self.y[n+1][i+1]\
                              + self.y[n+1][i-1]) - self.Mcr * (self.y[n+1][i+2] + self.y[n+1][i-2])
                self.y[n][0] = 0
                self.y[n][-1] = 0
        print len(self.y[2])
        
    def signal(self):
        self.sg = []
        for n in range(self.time):
            self.sg.append(self.y[n][int(0.05 * self.length)])
        
    def spectrum(self):
        self.p = abs(np.fft.rfft(self.sg))**2
        self.f = np.linspace(0, int(1/self.dt/2), len(self.p))
        
    def display(self):
        pl.figure()
        pl.xlabel('x', fontsize=10)
        pl.ylabel('y and t', fontsize=10)
        pl.title('Waves on a string (fixed ends)', fontsize=15)
        self.y = pl.array(self.y)
        add = pl.array([1 for i in range(self.length)])
        for n in range(0, 200, 20):
            yp = self.y[n] + add * n / 20
            pl.plot(self.x, yp)
       
        pl.figure()
        pl.plot(self.t, self.sg)
        pl.xlabel('time $(s)$', fontsize=10)
        pl.ylabel('Signal', fontsize=10)
        pl.title('String signal versus time', fontsize=15)
        
        pl.figure()
        pl.plot(self.f, self.p)
        pl.xlim(0, 4000)
        pl.xlabel('Frequency $(Hz)$', fontsize=10)
        pl.ylabel('Power (arbitrary units)', fontsize=10)
        pl.title('Power spectrum', fontsize=15)
       
a = string(1e-5)
a.update()
a.signal()
a.spectrum()
a.display()
```
