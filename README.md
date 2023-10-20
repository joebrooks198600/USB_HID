# USB_HID
/*
*Using C# HidLibrary in Hid Device
*This code is simple example for HidLibrary in C#, use the thread check what time insert the USB devce in background.
*
*/

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using HidLibrary;

namespace HidLibrary_ExampleConsoleApp1
{
    class Program
    {
        public static HidDevice device;
        private static bool isDeviceConnected = false;

        private const int VendorId = 0x1230; //usb hid vid
        private const int ProductId = 0xAAA; //usb hid pid

        static void Main(string[] args)
        {
            Thread deviceCheckThread = new Thread(() =>
            {
                while (true)
                {
                    try
                    {                        
                        HidDevice newDevice = HidDevices.Enumerate(VendorId, ProductId).FirstOrDefault();
                        bool isConnected = newDevice != null && newDevice.IsConnected;

                        //Check USB HID device is Connected or not，update the dvice state.
                        if (isDeviceConnected != isConnected)
                        {
                            isDeviceConnected = isConnected;
                            device = newDevice;

                            
                                if (isDeviceConnected)
                                {                                    
                                    Console.WriteLine("Device Connected.");
                                }
                                else
                                {
                                    Console.WriteLine("Device Removed.");
                                }
                            
                        }

                        Thread.Sleep(1000); // 1 second check one times.

                    }
                    catch (Exception ex)
                    {
                        // Handle exception
                        Console.WriteLine("Error Exception:", ex.Message);
                        
                        Thread.Sleep(1000); // 1 second check one times.
                    }
                }
            });
            deviceCheckThread.Start(); //Run thread check what time inser your USB HID in background.
        }
    }
}




