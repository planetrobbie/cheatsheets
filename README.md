Cheatsheets in this repository are supposed to be used as is by Cheaters app from [Brett Terpstra](http://brettterpstra.com/).

Start by cloning Cheaters repository

    git clone https://github.com/ttscoff/cheaters.git     

Clone this repo

    git clone https://github.com/planetrobbie/cheatsheets.git

And copy all markdown files into `cheaters/cheaters/` Cheaters directory where all cheatsheets should reside, and index in the server document root.
    
    cd cheatsheets
    cp *.md ../cheaters/cheaters/cheatsheets
    cp index.html ../cheaters/cheaters

Now serve the document root with Apache or any web browser, for example:

    cd ../cheaters/
    python -m SimpleHTTPServer 8080

Access it

    http://localhost:8080

Lastly if you're on a mac, and if you have $5 to spare, you can use [Fluid](http://fluidapp.com/) to create nice popup cheatsheets. Thanks Brett you've done a great job :) I love it.

For more info refer to [Cheaters documentation](https://github.com/ttscoff/).