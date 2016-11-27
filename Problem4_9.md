```python

import numpy as np 
import math as m
import pylab as pl
 

 
#Determine the initial value 
def initial(a,e): 
    x0=a*(1+e) 
    y0=0 
    v_x0=0 
    v_y0=2*m.pi*m.sqrt((1-e)/(a*(1+e))) 
    return [x0,y0,v_x0,v_y0] 

 
def orbits(beta, e): 
    i_M=initial(0.39, e) 
    x0=i_M[0] 
    x=[] 
    x.append(x0) 
    y0=i_M[1] 
    y=[] 
    y.append(y0) 
    v_x0=i_M[2] 
    v_x=[] 
    v_x.append(v_x0) 
    v_y0=i_M[3] 
    v_y=[] 
    v_y.append(v_y0) 
    r=[] 
    r.append(m.sqrt(x0**2+y0**2)) 
    time=1 
    dt=0.001 

 
    for i in range(int(time/dt)): 
        v_x.append(v_x[i]-4*m.pi**2*x[i]/(r[i]**(beta+1))*dt) 
        x.append(x[i]+v_x[i+1]*dt) 
        v_y.append(v_y[i]-4*m.pi**2*y[i]/(r[i]**(beta+1))*dt) 
        y.append(y[i]+v_y[i+1]*dt) 
        r.append(m.sqrt(x[i+1]**2+y[i+1]**2)) 
    return [x,y,v_x,v_y,r] 
 
 
#The orbits of planet 
 
M1=orbits(2.05, 0.1) 
x_M1=M1[0] 
y_M1=M1[1] 
M2=orbits(2.05, 0.3) 
x_M2=M2[0] 
y_M2=M2[1] 
M3=orbits(2.05, 0.5) 
x_M3=M3[0] 
`y_M3=M3[1] 
M4=orbits(2.05, 0.7) 
x_M4=M4[0] 
y_M4=M4[1] 

#plot 
origin1 = range(-100, 100)
origin2 = np.zeros(200)
pl.figure(figsize=[10,10])
 

pl.subplot(221)#3 numbers:the total number of rows,columns and the number of subplot 
pl.plot(x_M1,y_M1,color='grey') 
pl.scatter(0,0,s=1,color='red') 
pl.plot(origin2, origin1, ':', color = 'grey')
pl.plot(origin1, origin2, ':', color = 'grey')
pl.title('beta=2.05  e=0.1',fontsize=15) 
pl.xlabel('x/AU') 
pl.xlim(-0.6,0.8) 
pl.ylabel('y/AU') 
pl.ylim(-0.6,0.8) 

 
pl.subplot(222) 
pl.plot(x_M2,y_M2,color='grey') 
pl.scatter(0,0,s=1,color='red') 
pl.plot(origin2, origin1, ':', color = 'grey')
pl.plot(origin1, origin2, ':', color = 'grey')
pl.title('beta=2.05  e=0.3',fontsize=15) 
pl.xlabel('x/AU') 
pl.xlim(-0.6,0.8) 
pl.ylabel('y/AU') 
pl.ylim(-0.6,0.8) 

 
pl.subplot(223) 
pl.plot(x_M3,y_M3,color='grey') 
pl.scatter(0,0,s=1,color='red') 
pl.plot(origin2, origin1, ':', color = 'grey')
pl.plot(origin1, origin2, ':', color = 'grey')
pl.title('beta=2.05  e=0.5',fontsize=15) 
pl.xlabel('x/AU') 
pl.xlim(-0.6,0.8) 
pl.ylabel('y/AU') 
pl.ylim(-0.6,0.8) 

 
pl.subplot(224) 
pl.plot(x_M4,y_M4,color='grey') 
pl.scatter(0,0,s=1,color='red') 
pl.plot(origin2, origin1, ':', color = 'grey')
pl.plot(origin1, origin2, ':', color = 'grey')
pl.title('beta=2.05  e=0.7',fontsize=15) 
pl.xlabel('x/AU') 
pl.xlim(-0.6,0.8) 
pl.ylabel('y/AU') 
pl.ylim(-0.6,0.8) 

 
pl.savefig('planet2.png') 
pl.show() 
```
