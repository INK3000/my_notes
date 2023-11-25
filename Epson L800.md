**Look at the bottom for finally command**

To add a printer via `lpadmin` on a USB connection in a Unix-like system (such as Linux or macOS), you can follow these steps:

1. Connect the USB printer: Make sure your USB printer is properly connected to your computer and powered on.
    
2. Identify the printer: You'll need to know the name of the USB printer to use it with `lpadmin`. You can find this information using the `lsusb` command (Linux) or the "System Information" tool (macOS).
    
    - **Linux**: Run the following command to list the USB devices, including your printer:
        

- - `lsusb`
        
        Look for your printer's information, such as the vendor and product IDs.
        
- **Identify the Printer**: Connect the USB printer to your computer and make sure it's powered on.
    
- **Check for the Printer's Availability**: To ensure that your system recognizes the printer, open a terminal and use the `lpinfo` command to list available printers. This will help you confirm that the printer is detected:
    
    bash
    
- `lpinfo -v`
    
    Look for the USB-connected printer in the list, and note down its URI.
    
- **Add the Printer**: Now, you can use the `lpadmin` command to add the printer based on the detected URI. Replace `<printer_name>` with the name you want to give the printer and `<printer_uri>` with the URI you noted in the previous step.
    
    bash
    

`sudo lpadmin -p <printer_name> -E -v <printer_uri>`

For example:

bash

- `sudo lpadmin -p l800 -E -v usb://04b8:002a`
    
    This command adds the printer "l800" using the specified URI.
    
- **Specify the PPD File (Optional)**: If you have a specific PPD file for your printer, you can specify it when adding the printer. However, it's not always necessary, as the system may auto-detect and configure the printer correctly.
    
- **Set the Default Printer (Optional)**: If you want to set the newly added printer as the default printer, use the `lpoptions` command:
    
    bash
    
- `lpoptions -d <printer_name>`
    
    Replace `<printer_name>` with the name you assigned to your printer.
    
- **Print a Test Page**: You can use the `lp` command to print a test page and verify that the printer is working correctly:
    
    bash
    

1. `lp -d <printer_name> /etc/hosts`
    

Now, your USB printer should be added and ready to use on your Unix-like system. Be sure to replace `<printer_name>` and `<printer_uri>` with your specific printer name and URI.

## That's finally command

```bash
lpadmin -p l800 -E -v usb://EPSON/L800?serial=504B4E4B3130363323 -m epson-inkjet-printer-l800/EPSON_L800.ppd
```