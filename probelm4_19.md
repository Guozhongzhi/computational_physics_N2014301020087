```python
import math as m
import pylab as pl
 
#Determine the initial value 
def initial(a,e): 
    x0=a*(1+e) 
    y0=0 
    v_x0=0 
    v_y0=2*m.pi*m.sqrt((1-e)/(a*(1+e))) 
    return [x0,y0,v_x0,v_y0] 

 
def orbits(beta, e, theta_0): 
    i_M=initial(1, e) 
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
    time=8
    t = [0]
    dt=0.0001 
    #Hyperion motion
    theta = [theta_0]
    w =[0]

 
    for i in range(int(time/dt)): 
        v_x.append(v_x[i]-4*m.pi**2*x[i]/(r[i]**(beta+1))*dt) 
        x.append(x[i]+v_x[i+1]*dt) 
        v_y.append(v_y[i]-4*m.pi**2*y[i]/(r[i]**(beta+1))*dt) 
        y.append(y[i]+v_y[i+1]*dt) 
        r.append(m.sqrt(x[i+1]**2+y[i+1]**2)) 
        w.append(w[i] - (3 * 4 * m.pi ** 2 * (x[i] * m.sin(theta[i]) - y[i] * \
                        m.cos(theta[i])) * (x[i] * m.cos(theta[i]) + y[i] * \
                        m.sin(theta[i]))) * dt)
        theta.append(theta[i] + w[i + 1] * dt)
        if theta[-1] >= m.pi:
            theta[-1] = theta[-1] - 2 * m.pi
        if theta[-1] <= -m.pi:
            theta[-1] = theta[-1] + 2 * m.pi
        t.append(t[i] + dt)
    return [x,y,v_x,v_y,r,theta,w,t] 
 
def delta_theta(theta_1, theta_2):
    delta_theta = []
    for i in range(len(theta_1)):
        delta_theta.append(abs(theta_1[i] - theta_2[i]))
    return delta_theta
    

#The orbits of moon 


M5 = orbits(2.0, 0, 0.004)
theta_M5 = M5[5]
w_M5 = M5[6]
t_M5 = M5[7]

M6 = orbits(2.0, 0, 0.003)
theta_M6 = M6[5]
w_M6 = M6[6]
t_M6 = M6[7]

delta_theta_M = delta_theta(theta_M6, theta_M5)
everage_M = sum(delta_theta_M) / len(delta_theta_M)#Lyapunov exponent
print everage_M
Delta_theta = []
for i in range(len(delta_theta_M)):
    Delta_theta.append(m.exp(everage_M * t_M5[i]))
pl.figure(figsize=[10,5])
pl.subplot(121)
pl.title('Hyperion  $\Delta\\theta$  versus  time',fontsize=15)
pl.xlabel('time $(yr)$',fontsize=15)
pl.ylabel('$\Delta\\theta$ (radius)',fontsize=15)
pl.text(3, 0.06, 'Circular orbit', fontsize=15)
pl.plot(t_M5, delta_theta_M)
pl.plot(t_M5, Delta_theta)
pl.semilogy(0.0001, 0.1)

M7 = orbits(2.0, 0.28, 0.004)
theta_M7 = M7[5]
w_M7 = M7[6]
t_M7 = M7[7]

M8 = orbits(2.0, 0.28, 0.003)
theta_M8 = M8[5]
w_M8 = M8[6]
t_M8 = M8[7]

delta_theta_M = delta_theta(theta_M8, theta_M7)
everage_M = sum(delta_theta_M) / len(delta_theta_M)
Delta_theta = []
for i in range(len(delta_theta_M)):
    Delta_theta.append(m.exp(everage_M * t_M7[i]))
print everage_M
pl.subplot(122)
pl.title('Hyperion  $\Delta\\theta$  versus  time',fontsize=15)
pl.xlabel('time $(yr)$',fontsize=15)
pl.ylabel('$\Delta\\theta$ (radius)',fontsize=15)
pl.text(1, 10 ** 3, 'Elliptical orbit', fontsize=15)
pl.plot(t_M7, delta_theta_M)
pl.plot(t_M7, Delta_theta)
pl.semilogy(0.0001, 0.1)
```
