# OSINT

Tags: #OSINT

## Intelligence Lyfecycle 

```bash 
1. Planning and direction 
2. Collection 
3. Processing and exploitation 
4. Analysis and production 
5. Dissemination and integration 
```

## Creating Sock Puppets

```bash 
❯ https://web.archive.org/web/20210125191016/https://jakecreps.com/2018/11/02/sock-puppets/    # Crear un 'Sock Puppet'
❯ https://www.secjuice.com/the-art-of-the-sock-osint-humint/    # The Art Of The Sock
❯ https://www.reddit.com/r/OSINT/comments/dp70jr/my_process_for_setting_up_anonymous_sockpuppet/?rdt=47120 # Reddit

❯ https://www.fakenamegenerator.com/           # Fake Name Generator
❯ https://www.thispersondoesnotexist.com/      # This Person Does not Exist
❯ https://privacy.com/referral                 # Privacy.com
```

## Search Engine Operators

```bash 
❯ https://www.google.com/
❯ https://www.google.com/advanced_search

❯ https://www.bing.com/
❯ https://www.bruceclay.com/blog/bing-google-advanced-search-operators/

❯ https://duckduckgo.com/
❯ https://duckduckgo.com/duckduckgo-help-pages/results/syntax/

❯ https://www.baidu.com/

❯ https://yandex.com/
```

## Google Dorks

```
❯ https://www.googleguide.com/print/adv_op_ref.pdf

site:reddit.com             # Obtendremos resultados con ese sitio web en especifivo
	site:reddit.com -www   # Obtendremos resultados de 'Subdominios', con '-' quitamos los resultados que no queremos
filetype:pdf                # Obtendremos resultados con archivos 'pdf' 
intext:password             # Obtendremos resultados en donde tendremos la palabra 'password' en el texto 
inurl:password              # Obtendremos resultados en donde tendremos la palabra 'password' en la url 

AND                         # Obtendremos resultados mas especificos con el operador 'AND' 
OR                          # Obtendremos diferentes resultados con la condicion del operador 'OR'
" "                         # Resultados especificos con " "
* word                      # Wildcard, hace match con todo lo que encuentre antes de la palabra 'word'
```