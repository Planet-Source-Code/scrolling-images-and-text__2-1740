<div align="center">

## Scrolling images and text


</div>

### Description

Java Applet that displays scrolling images and text.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[N/A](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/empty.md)
**Level**          |Beginner
**User Rating**    |2.7 (8 globes from 3 users)
**Compatibility**  |Java \(JDK 1\.1\)
**Category**       |[Graphics](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/graphics__2-75.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/scrolling-images-and-text__2-1740/archive/master.zip)





### Source Code

```
import java.applet.*;
import java.awt.*;
import java.awt.image.PixelGrabber;
import java.awt.image.MemoryImageSource;
public class CoolScroll extends Applet implements Runnable
{
private Thread m_CoolScroll = null;
boolean asodreadisegna = false;
private double rad = 20.0,
rad_inc = 0.5,
RAD = 3.1415926535 / 180,
ang_x = 0.0,
angolo_x = 0.0,
inc_factor = 1.5;
int asciioffset = 0;
int veloce;
char msg[];
int msgbounceindex [];
int lametable[];
int xadd = 0;
int scrollpos = 0;
int mirrorpic[],
swappic[];
int startx = 0,
starty = 0,
msglenght;
Color forecolor,
backcolor;
Image backdrop;
Image appletimage;
Image cropfont,
upperimage,
lowerimage;
int redmaskadd = 90,
greenmaskadd = 120,
bluemaskadd = 140;
int redadd = 5,
greenadd = 5,
blueadd = 5;
Image letter;
int extent_x,
extent_y;
int letterwidth = 0,
letterheight = 0;
private MemoryImageSource screenMem;
Graphics gfx,
lettergfx,
appletgfx;
public void init()
{
String messaggio;
messaggio = getParameter("scrolltext");
if(messaggio == null)
{
messaggio="SAY SOMETHING TO THE WORLD...WRITE IT!";
}
veloce = Integer.parseInt(getParameter("scrollspeed"));
asciioffset = Integer.parseInt(getParameter("asciioffset"));
//Single letter build
letterwidth = Integer.parseInt(getParameter("fontx"));
letterheight = Integer.parseInt(getParameter("fonty"));
letter = createImage(letterwidth , letterheight );
lettergfx = letter.getGraphics();
//Single letter build
appletimage = createImage(size().width , size().height);
appletgfx = appletimage.getGraphics();
//Half scroll build
extent_x = size().width / 2;
extent_y = size().height / 4;
upperimage = createImage( extent_x , extent_y );
gfx = upperimage.getGraphics();
scrollpos = extent_x;
swappic = new int[extent_x * extent_y + extent_x];
mirrorpic = new int[extent_x * extent_y + extent_x];
//Half scroll build
//ScrollText build
msglenght = messaggio.length();
msg = new char[msglenght+1];
//Get ready to bounce...
msgbounceindex = new int[msglenght+1];
lametable = new int[16];
loadlameBounce();
int nowind = 0;
for (int loop=0; loop < msglenght; loop++)
{
msgbounceindex[loop] = nowind;
if(++nowind>15)
nowind = 0;
}
msg = messaggio.toCharArray();
//ScrollText build
MediaTracker tracker = new MediaTracker(this);
cropfont = getImage(getDocumentBase(), getParameter("fontimage"));
tracker.addImage(cropfont, 0);
try
{
tracker.waitForID(0);
}
catch (InterruptedException e)
{
}
MediaTracker tracker2 = new MediaTracker(this);
backdrop = getImage(getDocumentBase(), getParameter("backimage"));
tracker2.addImage(backdrop, 1);
try
{
tracker2.waitForID(1);
}
catch (InterruptedException e)
{
}
forecolor = new Color(255,0,0);
backcolor = new Color(0,0,0);
screenMem = new MemoryImageSource(extent_x,
extent_y,
mirrorpic,
0,
extent_x);
}
public void update(Graphics g)
{
gfx.drawImage(backdrop,0,0,this);
BounceScroll();
loadPix();
renderPix();
appletgfx.drawImage(upperimage,0,0, extent_x * 2, extent_y * 2, this);
appletgfx.drawImage(lowerimage,0,extent_y*2, extent_x * 2, (extent_y * 2)+xadd, this);
paint(g);
}
public void paint(Graphics g)
{
if (appletgfx != null)
{
g.drawImage(appletimage ,0,0, this);
}
}
public void start()
{
if (m_CoolScroll == null)
{
m_CoolScroll = new Thread(this);
m_CoolScroll.start();
}
}
public void stop()
{
if (m_CoolScroll != null)
{
m_CoolScroll.stop();
m_CoolScroll = null;
}
}
public void run()
{
while (true)
{
try
{
repaint();
Thread.sleep(50);
System.gc();
}
catch (InterruptedException e)
{
stop();
}
}
}
private void loadPix()
{
int punto = 0;
int themask = 0;
asodreadisegna = true;
PixelGrabber grabber = new PixelGrabber( upperimage,
0,
0,
extent_x,
extent_y,
swappic ,
0,
extent_x);
boolean done = false;
do
{
try
{
done = grabber.grabPixels( 500 );
}
catch ( InterruptedException e ) {}
}
while( !done );
asodreadisegna = false;
}
private void renderPix()
{
int punto = 0;
int themask = 0;
int red = 0;
int green = 0;
int blue = 0;
int yindex = extent_y-1;
redmaskadd += redadd;
greenmaskadd += greenadd;
bluemaskadd += blueadd;
if(redmaskadd == 255 || redmaskadd == 90 )
redadd = redadd * -1;
if(greenmaskadd == 255 || greenmaskadd == 90 )
greenadd = greenadd * -1;
if(bluemaskadd == 255 || bluemaskadd == 90 )
blueadd = blueadd * -1;
for(int loopy = 0; loopy < extent_y; loopy++)
{
for(int loopx = 0; loopx < extent_x; loopx++)
{
punto = swappic[extent_x * yindex + loopx];
themask = punto & 0x00ff0000;
red = themask >> 16;
themask = punto & 0x0000ff00;
green = themask >> 8;
themask = punto & 0x000000ff;
blue = themask;
//red = blue;
//green = blue;
red = red;
green = green;
red = (int)((red * redmaskadd)<<8);
green = (int)((green * greenmaskadd));
blue = (int)((blue * bluemaskadd)>>8);
mirrorpic[extent_x * loopy + loopx] = 0xff000000 |(red & 0x00ff0000) | (green & 0x0000ff00) | (blue & 0xff);
}
yindex--;
}
lowerimage = createImage(screenMem);
}
// Lame Pre-Calc bounce table....
private void loadlameBounce()
{
lametable[0] = 0;
lametable[1] = 0;
lametable[2] = 0;
lametable[3] = 1;
lametable[4] = 1;
lametable[5] = 2;
lametable[6] = 4;
lametable[7] = 6;
lametable[8] = 10;
lametable[9] = 15;
lametable[10] = 10;
lametable[11] = 6;
lametable[12] = 4;
lametable[13] = 2;
lametable[14] = 1;
lametable[15] = 1;
}
private void BounceScroll()
{
int retval = 0;
double CX;
for (int loopx = 0; loopx < msglenght; loopx++)
{
if( scrollpos + (loopx * letterwidth ) < extent_x && scrollpos + (loopx * letterwidth ) > letterwidth * -1)
{
getletterIndex((int)msg[loopx]);
//Simulating transparent color....
lettergfx.drawImage(backdrop, -(scrollpos + (loopx * letterwidth)), -(1+lametable [msgbounceindex[loopx]]),this);
lettergfx.drawImage(cropfont, -(startx * letterwidth), -(starty * letterheight),this);
gfx.drawImage(letter,scrollpos + (loopx * letterwidth),1+lametable [msgbounceindex[loopx]],this);
lettergfx.fillRect(0,0,letterwidth, letterheight);
}
if(++msgbounceindex[loopx] > 15)
msgbounceindex[loopx] = 0;
}
scrollpos -= veloce;
if(scrollpos * -1 > msglenght * letterwidth)
scrollpos = extent_x;
//sinus stuff
angolo_x += inc_factor;
if (angolo_x > 90 || angolo_x < 0)
{
inc_factor = inc_factor * -1;
}
CX = Math.cos((angolo_x ) * RAD);
xadd = (int)(angolo_x * CX);
//end of sinus stuff
}
private void getletterIndex(int ASCIIcode)
{
int retval = 0;
retval = ((int)ASCIIcode)-asciioffset;
if(retval<=0)
{
startx = 0;
starty = 0;
}
else
{
starty = ((int)(retval / 10));
startx = (retval%10);
}
}
}
```

