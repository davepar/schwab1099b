# Schwab 1099-B Converter [rsulove.com or schwab1099b.com]

The purpose of these two scripts is to automate entering stock sales from the
Schwab EAC 1099-B form into TurboTax Online.

There are several steps to this process, but it does work. And it can save a
lot of time trying to enter the transaction manually.

1. Log into the Schwab EAC and then visit each of these links to download a PDF
   of the 1099-B forms. You may only have one of these or both:

   * https://eac.schwab.com/eac/myawards/eDocuments/taxForms.do?tabSource=statTab&linkSource=taxForms&psymbol=GOOGL
   * https://eac.schwab.com/eac/myawards/eDocuments/taxForms.do?tabSource=statTab&linkSource=taxForms&psymbol=GOOG

2. For each PDF, select some of the contents, use cmd-A to select everything, the copy with cmd-C.
   Open a text editor and paste the contents into a plain text file and save as something like
   `goog-1099b.txt`.

3. Run the Python script in this repository against the files. If you have two files, you should
   process them both at the same time:

   `$ ./convert-1099b-json.py goog-1099b.txt googl-1099b.txt > entries.json`

   Some totals will be printed out that you should compare to the original 1099-B forms to make
   sure all transactions were processed.
   You will now a file entries.json containing all of your GOOG and GOOGL
   transactions. The number of transactions of each type is also printed.

4. The next part of the process takes part in the browser on turbotax.com. I used
   Google Chrome on MacOS. It should work with other browsers, but I didn't try it.
   Open the TurboTax site and navigate to the section for entering stock transactions.
   Enter your instituion name ("Charles Schwab") and account number, and eventually you'll
   arrive at a screen that says:

   > Tell us about your Charles Schwab 1099-B
   > * I'll enter one sale at a time
   > * I'll enter a summary for each sales category
   
5. Once you're on the page at step 4, open the Developer Tools window (Settings
   -> More Tools). Click on the "Console" tab if it's not already selected.

6. Copy the contents of the `enter.js` file in this repository, click your cursor into the console
   area of the browser, and paste. This won't take any action other than defining a bunch of
   functions.

7. Now do the same for the contents of the `entries.json` file created above. Copy the contents.
   Paste it into the console window.

8. Now the fun begins. In the console, type:

   `> enterAll(entries)`

   *Very important!* Immediately after typing this, click somewhere in the TT page to give it focus
   again. (Don't click on a link or a form entry; the best place to click is the empty gray
   margins.) You only need to do it once at the beginning, before the script starts filling out the
   entries.

9. Now sit back and watch the data get filled out. *Don't* switch tabs or windows. It's a good idea
   to move your mouse every few minutes to keep your screen from going to sleep.

10. You can always navigate back to the screen described in step 4 and re-enter the command from
    step 8 if you need to add more entries.

11. That's it. Hopefully all of your transactions are now entered into TurboTax.
