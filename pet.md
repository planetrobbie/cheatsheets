{
    "id": "auto",
    "style": "css/h4dark.css",
    "layout": "default"
}
%%%END

# Pet
Simple command-line snippet manager, written in Go.

### Commands

cli | description
-- | -- | --
pet search or `xCTRLs` | Search for a command
pet new -t | Create a new command with a tags space separated
pet edit | Edit Commands
pet list | List all existing commands
pet exec | Execute a command
pet sync | Sync up to the configured Gist
pet configure | Configure

### Tips & Tricks

Variables can be used in commands between &lt; and &gt; like this

    git remote add origin git@github.com:planetrobbie/&lt;REPO&gt;.git

Add this function to you `.bashrc` or `.zshrc` to easily add previous command as a Pet snippet.

    function prev() {
      PREV=$(fc -lrn | head -n 1)
      sh -c "pet new `printf %q "$PREV"`"
    }

Add this code to your `.zshrc` to search using `xCTRL-s`
    
    function pet-select() {
      BUFFER=$(pet search --query "$LBUFFER")
      CURSOR=$#BUFFER
      zle redisplay
    }
    zle -N pet-select
    stty -ixon
    bindkey '^s' pet-select

By using `pbcopy` on OS X, you can copy snippets to clipboard to run a command remotely using a SSH connection.

    pet search | pbcopy

Snippets are locally stored but can be saved in a Gist

    vi ~/.config/pet/snippet.toml

Get an access token from [GitHub](https://github.com/settings/tokens/new) and set that in Pet configuration

    [Gist]
      file_name = "pet.toml"
      access_token = "&lt;TOKEN&gt;"
      gist_id = "&lt;GIST_ID&gt;"
      public = false
      auto_sync = false

If you set `auto_sync` to true, Pet will be automatically sync'ed after addition.

---

Cheatsheet freely available on [GitHub](source available on [GitHub](https://github.com/planetrobbie/cheatsheets/pet.md), created by planetrobbie.
**Pet** is available on [https://github.com/knqyf263/pet](GitHub)