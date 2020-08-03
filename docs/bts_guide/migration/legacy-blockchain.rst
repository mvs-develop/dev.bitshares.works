******************************************
Legacy DNA 0.9.3 Blockchain Download
******************************************

One crucial step in teh migration still takes quite some time and is
technically invovled for those that do **not** see their funds in
DNA 0.9.3: The need to download the blockchain from the legacy
network. Since only a few nodes still serve the raw blockchain data, we
here provide the full blockchain to download:

:download:`Blockchain Torrent <DNA-0.9.3-full-blockchain.torrent>`

Usage
-----

1. Download the Torrent and use it to obtain a full copy of the `chain/` folder
2. Move this chain folder into your DNA shared directory:

   * Windows: `%APPDATA%/DNA`
   * Mac: `~/Library/Application Support/DNA`
   * Linux: `~/.DNA`

3. Start DNA 0.9.3 (not older) and let it re-index the database (be patient)
4. Provide your Passphrase
5. Goto `account list->advanced settings->console` and run `rescan`. Depending on the amount of accounts in your wallet, this steps should only take very few minutes. You can see the syncing progress from the status bar or from the info command in the console.
6. After completion, you should now see your funds again and can continue with exporting your wallet for DNA 2

Verification
------------

::

     $ md5sum DNA-0.9.3-full-blockchain.torrent
     bf62712aeb02e7eb3619e4539d1aefcf  DNA-0.9.3-full-blockchain.torrent

     $ sha1sum DNA-0.9.3-full-blockchain.torrent
     249f52c57c4169ce898fdf3e8848d725e942a581  DNA-0.9.3-full-blockchain.torrent

     $ sha512sum DNA-0.9.3-full-blockchain.torrent
     7626bc38f722f86e6859332274e3090ce2360c9978a02db0bbdcc7352fbf1df7 \
     924901c3c3c92cdb7a9797e9608a5d0ef5ebfa9ee165d435048bf380c45ec555  DNA-0.9.3-full-blockchain.torrent
