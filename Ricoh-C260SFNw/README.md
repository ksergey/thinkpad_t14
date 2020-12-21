# Configure network printer Ricoh SP C260SFNw on my thinkpad

Let's install packages:
```
pacman -S cups ghostscript system-config-printer
```

Check your regular user contains group `cups` (use `id`)

Let's add printer via `system-config-printer`.
Printer url should looks like `ipp://[printer ip or name]:631/ipp/print`.
When prompt for printer model select my [ppd](Ricoh-SP-C260SFNw.ppd). I think you could choose something else from foomatic-db or ricoh generic driver.

Done!
