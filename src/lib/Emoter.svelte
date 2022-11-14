<script>
  import Foswig from 'foswig';
  let bttv = []
  let created = []
  function fosGen() {
    for (let i=0; i<25; i++) {
      try {
        let memote = fos.generate(constraints)
        if (created.includes(memote)) {
          continue
        }
        created.push(memote)
        emote = memote
        return
      } catch(e) {
        console.log(e)
      }
    }
  }
  const constraints = { 
  maxLength: 16, 
  allowDuplicates: false
  };
  let fos
  function downloadURI(uri, name) {
    var link = document.createElement("a");
    link.download = name;
    link.href = uri;
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  }
  fetch("https://api.betterttv.net/3/cached/emotes/global").then(async (data) => {
    let d = await data.json()
    d.forEach((i) => {
      bttv.push(i.code)
    })
    fos = new Foswig(2, bttv)
    setEmote()
  })
  let emote = ""
  function setEmote() {
    fosGen()
  }
  function downloadEmoteList() {
    let text = created.join("\n")
    downloadURI('data:text/plain;charset=utf-8,' + encodeURIComponent(text), "twitch.txt")
  }
</script>

<h2>
  {emote}
</h2>

<button on:click={setEmote}>
  Again!
</button>
<button on:click={downloadEmoteList}>
  Download all
</button>

<style>
  button {
    font-family: inherit;
    font-size: inherit;
    padding: 1em 2em;
    color: #ff3e00;
    background-color: rgba(255, 62, 0, 0.1);
    border-radius: 2em;
    border: 2px solid rgba(255, 62, 0, 0);
    outline: none;
    width: 200px;
    font-variant-numeric: tabular-nums;
    cursor: pointer;
  }

  button:focus {
    border: 2px solid #ff3e00;
  }

  button:active {
    background-color: rgba(255, 62, 0, 0.2);
  }
</style>
