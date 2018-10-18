using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Media;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Threading;
using System.Windows.Shapes;

namespace Trabajo_en_clase_5
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        int iPixeles = 3;
        double iAnchoCanvas,iAltoCanvas;

      
        int iNumImagen = 1;
        double dAnchoCanvas, dAnchoFondo;
        double dPosicionFondo;
        int iPixles = 5;
        int iNumDisco = 1;

        DispatcherTimer timer;


        struct Objeto
        {
            public double dPosX;
            public double dPosY;
            public double dAncho;
            public double dAlto;
        }

        Objeto obR,obM,obA;

       

        public MainWindow()
        {
            InitializeComponent();

            MainCanvas.Focusable = true;
            MainCanvas.Focus();

            iAnchoCanvas = MainCanvas.Width;
            iAltoCanvas = MainCanvas.Height;

            obA.dPosX = (double)objetoA.GetValue(Canvas.LeftProperty);
            obA.dPosY = (double)objetoA.GetValue(Canvas.TopProperty);
            obA.dAncho = objetoA.Width;
            obA.dAlto = objetoA.Height;

            obR.dAncho = objetoR.Width;
            obR.dAlto = objetoR.Height;


            timer = new DispatcherTimer();
            timer.Interval = new TimeSpan(0, 0, 0, 0, 30);
            timer.Tick += new EventHandler(Timer_Tick);
            timer.IsEnabled = true;

            timer = new DispatcherTimer();
            timer.Interval = new TimeSpan(0, 0, 0, 0, 200);
            timer.Tick += new EventHandler(Timer_Tick2);
            timer.IsEnabled = true;
        }

        private void MainCanvas_KeyDown(object sender, KeyEventArgs e)
        {
            obR.dPosX = (double)objetoR.GetValue(Canvas.LeftProperty);
            obR.dPosY = (double)objetoR.GetValue(Canvas.TopProperty);

            obM.dPosX = (double)objetoM.GetValue(Canvas.LeftProperty);
            obM.dPosY = (double)objetoM.GetValue(Canvas.TopProperty);

            switch (e.Key)
            {
                case Key.Left:
                    if (obR.dPosX - iPixeles > 0)
                        obR.dPosX -= iPixeles;
                    break;

                case Key.Right:
                    if (obR.dPosX + iPixeles + objetoR.Width < iAnchoCanvas)
                        obR.dPosX += iPixeles;
                    break;

                //Nva coord arriva
                case Key.Up:
                    if (obR.dPosY - iPixeles > 0)
                        obR.dPosY -= iPixeles;
                    break;
                //determina hacia abajo
                case Key.Down:
                    if (obR.dPosY + iPixeles + objetoR.Height < iAltoCanvas)
                        obR.dPosY += iPixeles;
                    break;
            }
            if (checarColision(obR, obA))
                SystemSounds.Hand.Play();
            else
            {
                objetoR.SetValue(Canvas.LeftProperty, obR.dPosX);
                objetoR.SetValue(Canvas.TopProperty, obR.dPosY);
            }

            if (checarColision2(obR, obA))
                SystemSounds.Hand.Play();
            else
            {
                objetoR.SetValue(Canvas.LeftProperty, obR.dPosX);
                objetoR.SetValue(Canvas.TopProperty, obR.dPosY);
            }
        }
            private bool checarColision (Objeto ob1, Objeto ob2)
        {
            if (ob1.dPosX + ob1.dAncho < ob2.dPosX)
                return false;
            if (ob1.dPosY + ob1.dAlto < ob2.dPosY)
                return false;
            if (ob1.dPosY > ob2.dPosY + ob2.dAlto)
                return false;
            if (ob1.dPosX > ob2.dPosX + ob2.dAncho)
                return false;
            return true;
        }

        private bool checarColision2(Objeto ob1, Objeto meta)
        {
            if (ob1.dPosX + ob1.dAncho < meta.dPosX)
                return false;
            if (ob1.dPosY + ob1.dAlto < meta.dPosY)
                return false;
            if (ob1.dPosY > meta.dPosY + meta.dAlto)
                return false;
            if (ob1.dPosX > meta.dPosX + meta.dAncho)
                return false;
            return true;
        }
        private void Timer_Tick(object sender, EventArgs e)
        {
            dPosicionFondo = (double)fondo.GetValue(Canvas.LeftProperty);

            if (dPosicionFondo + dAnchoFondo + iPixeles > dAnchoCanvas)
            {
                fondo.SetValue(Canvas.LeftProperty, dPosicionFondo - iPixeles);
               
            }
           
        }
        private void Timer_Tick2(object sender, EventArgs e)
        {
            dPosicionFondo = (double)meta.GetValue(Canvas.LeftProperty);

            if (dPosicionFondo + dAnchoFondo + iPixeles > dAnchoCanvas)
            {
                meta.SetValue(Canvas.LeftProperty, dPosicionFondo - iPixeles);
               
            }
            
        }
    }
    }

