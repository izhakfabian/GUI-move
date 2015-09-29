# GUI-move
import numpy as np
import matplotlib.pyplot as plt
from sklearn import tree
import matplotlib.patches as patches
fig = plt.figure()
plt.axis("equal", polar=True)
plt.axis([0,40, 0,40])
plt.grid(True)

circ=plt.Circle((30,30),8,color='r')

ax = fig.add_subplot(111)
ax.add_patch(circ)
class EventHandler:
   def __init__(self):
       fig.canvas.mpl_connect('button_press_event', self.onpress)
       fig.canvas.mpl_connect('button_release_event', self.onrelease)
       fig.canvas.mpl_connect('button_release_event', self.calculate)
       fig.canvas.mpl_connect('motion_notify_event', self.onmove)
       self.x0, self.y0 = circ.center
       self.pressevent = None

   def onpress(self, event):
      if event.inaxes!=ax:
         return

      if not circ.contains(event)[0]:
         return

      self.pressevent = event

   def onrelease(self, event):
      self.pressevent = None
      self.x0, self.y0 = circ.center
      x=self.x0
      y=self.y0
      r=np.sqrt((x-20)**2+(y-20)**2)
      percentag= (r/8)*100
      rad=(np.arctan2(y-20, x-20)-np.pi/2)
      phi = np.rad2deg(rad)
      if phi<=0 and phi >=-270:
          phi=-phi
      elif  phi>=0 and phi <=90:
          phi=-phi+360
      return phi, percentag

   def calculate(self, event):
       x= list(EventHandler.onrelease(self, event))
       t= x[0]
       print t




   def onmove(self, event):
      if self.pressevent is None or event.inaxes!=self.pressevent.inaxes:
         return

      dx = event.xdata - self.pressevent.xdata
      dy = event.ydata - self.pressevent.ydata
      circ.center = self.x0 + dx, self.y0 + dy

      fig.canvas.draw()


handler = EventHandler()


plt.show()
