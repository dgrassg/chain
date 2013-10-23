chain
=====
import numpy as np
import pylab as pl

P1=[]
n=10
for i in range(n):
    P1.append([])
    P1[i].append(1.0-np.float(np.float(1)/np.float(i+2)))
    P1[i].append(np.float(np.float(1)/np.float(i+2)))
    P1[i].append(1.0-np.float(np.float(i+1)/np.float(i+2)))
    P1[i].append(np.float(np.float(i+1)/np.float(i+2)))    
VP1= np.asarray(P1)
VP1=VP1.reshape(n,4)


def potencia(x):
  if x==0 :
    return 1
  else:
    return 2 * potencia(x-1)

def calculaprobabilidad(P, n, x0, xf, X, Xn):
   t=0
   band=0
   exp = potencia(n)
   mat = np.empty((exp,n))
   #genera las combinaciones (0,1) posibles en n
   itera=exp/2
   for j in range(n):                                         
     for i in range(exp):
         if band==0:
             mat[i][j]=0
             t=t+1             
         else:
             mat[i][j]=1
             t=t-1
         if t==itera:
             band = 1
         if(t==0):
             band = 0
     itera=itera/2
   #Ahora se seleccionan de las combinaciones todos los que coinciden con la busqueda
   Combina=[]
   h=0
   for i in range(exp):     
     if mat[i][0] == x0 and mat[i][Xn] == X and mat[i][n-1] == xf:
         Combina.append([])    
         for j in range(n):             
             Combina[h].append(mat[i][j])
         h=h+1
   Combina=np.asarray(Combina)
   #Calculo la sumatoria con la productoria
   suma=0
   
   for i in range(h):
       prod=1
       f=1
       for j in range(n-1):
           if Combina[i][f-1]==0 and Combina[i][f]==0:
              prod=prod*P[f][0]
           if Combina[i][f-1]==0 and Combina[i][f]==1:
              prod=prod*P[f][1]
           if Combina[i][f-1]==1 and Combina[i][f]==0:
              prod=prod*P[f][2]
           if Combina[i][f-1]==1 and Combina[i][f]==1:
              prod=prod*P[f][3]
           f=f+1
       suma=suma+prod 
   return suma

    
y1= calculaprobabilidad(VP1, n, 1, 0, 0, n/2)*0.5
y2= calculaprobabilidad(VP1, n, 1, 0, 1, n/2)*0.5
y= np.float(y1+y2)
p1=np.float(np.float(y1)/np.float(y))
p2=np.float(np.float(y2)/np.float(y))

print p1
print p2
