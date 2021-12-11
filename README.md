## Blockchain Notes

This is a repo that stores my notes of blockchain.

### Tips

For compiling `md` to `pdf` with `bib` and `table of content`, refer to the following sample code:

`pandoc --bibliography refs.bib --metadata link-citations=true --citeproc --csl ieee.csl .\exchange_concepts.md  --toc -o exchange_concepts.pdf`