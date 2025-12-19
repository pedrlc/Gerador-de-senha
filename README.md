## Gerador de senha segura    

Projeto front-end desenvolvido com HTML, CSS e JavaScript puro, que permite gerar senhas aleatórias e seguras de forma dinâmica. O usuário pode personalizar o tamanho da senha e os tipos de caracteres utilizados (maiúsculas, números e símbolos). O sistema inclui um indicador visual de força da senha, atualização em tempo real via slider e funcionalidade de cópia automática para a área de transferência. 


O script desse código possue comentários para o entendimento da lógica de programação usada no JavaScript bruto do projeto.

---

##  Tecnologias Utilizadas

- HTML5
- CSS3
- JavaScript (Vanilla)
- API Clipboard do navegador

---
##  Funcionalidades

- Geração de senhas aleatórias
- Definição do tamanho da senha (4 a 64 caracteres)
- Opção de uso de:
  - Letras maiúsculas
  - Números
  - Símbolos
- Indicador visual de força da senha
- Ajuste automático do tamanho da fonte conforme o comprimento da senha
- Botão para copiar a senha para a área de transferência
- Interface responsiva (desktop e mobile)

---
## Código em JavaScript   
```js
const inputEl = document.querySelector("#password")
    const upperCasecheckEl = document.querySelector("#uppercase-check")
    const NumbercheckEl = document.querySelector("#numbers-check")
    const symbolcheckEl = document.querySelector("#symbol-check")
    const securityIndicatorBarEl = document.querySelector("#security-indicator-bar")
    let passwordLength = 16
    
    

    // essa função irá gerar uma senha aleatória para o usuário
    function generatePassword() {
        let chars = 
        "abcdefghjkmnpqrstuvwxyz"
        // todos os caracteres que poderão ser sorteados na geração da senha

        const uperCaseChars = "ABCDEFGHJKLMNPQRSTUVWXYZ"

        const numbersChars = "123456789"
        
        const specialChars = "?!@&*()[]"

        if(upperCasecheckEl.checked) {
            chars += uperCaseChars    
        }
        
        if(NumbercheckEl.checked) {
            chars += numbersChars

        }
        if(symbolcheckEl.checked) {
            chars += specialChars

        }
        /* os ifs conferem se as seguintes variaveis estão checkadas, caso estejam o as constantes criadas
        entram dentro da variavel chars */

        let password = ""
        // variavel vazia que será usada para construir uma senha final

        for(let i = 0; i < passwordLength; i++) {
            // loop inicialmente irá repetir 8 vezes, garantindo que a senha tenha 8 caracteres
            const randomNumber = Math.floor(Math.random() * chars.length)
            password += chars.substring(randomNumber, randomNumber + 1) // recorte da string chars
            /*
            math.random() gera um numero entre 0 e 1 e ele será multiplicado pelo tamanho
            total de chars, isso fará que um nuemro aleatória seja escolhido.
            Logo após isso o chars.substring irá até o numero do caractere sorteado, pegando ele
            e usando randomNumber + 1 como parâmetro para pegar o caractere da posição sorteada até o 
            caractere posterior a ele. Isso tudo será repetido 8 vezes, pegando assim 8 caracteres aleatorios
            e colocando todos eles na variavel password.
            */
        }
        
        
        inputEl.value = password
        calculateQuality()
        calculatefontSize()
        /*
        document.querySelector irá buscar em todo o documento um elemento que tenha o 
        atributo id igual a password. Logo o atributo .value armazena o conteudo do campo 
        input e armazena o password gerado la dentro
        */
    }

    function calculateQuality() {
        // 20% critico // 100% safe
        // 64/64 = 1 -> 100%

        const percentual = Math.round((passwordLength / 64 ) * 100 * 0.25 +
        (upperCasecheckEl.checked ? 15 : 0) +
        (NumbercheckEl.checked ? 25: 0) + 
        (symbolcheckEl.checked ? 35 : 0)
    )
        console.log(percentual)

        securityIndicatorBarEl.style.width = `${percentual}%`

        if (percentual > 69 ) {
            securityIndicatorBarEl.classList.remove("critical")
            securityIndicatorBarEl.classList.remove("warning")
            securityIndicatorBarEl.classList.add("safe")

        } else if (percentual > 50) {
            securityIndicatorBarEl.classList.remove("critical")
            securityIndicatorBarEl.classList.add("warning")
            securityIndicatorBarEl.classList.remove("safe")
        } else {
            securityIndicatorBarEl.classList.add("critical")
            securityIndicatorBarEl.classList.remove("warning")
            securityIndicatorBarEl.classList.remove("safe")
        }

        if (percentual >= 100) {
            securityIndicatorBarEl.classList.add("completed")
        } else {
            securityIndicatorBarEl.classList.remove("completed")
        }
        /* esses ifs classificam sao responsaveis pelo estado visual da barra de seguranca com base no percentual
        safe > 69, warning > 50, critical <50. para cada caso sao adcionadas e removidas as classes css
        usando o classList, garantindo que cada cor esteja ativo por vez de acordo com a condição. Por ultimo o é feito
        uma verificação de quando a senha atinge os 100%, quando isso aconteçe a classe completed é ativada
        indicando visualmente com uma pequena alteração na borda da barra de segurança que chegou ao fim(100%)
        */

        /*funcao que calcula oa forca da senha em forma porcentual 0-100% com base nos criteterios de controle de senha
        cada controle de senha tem seu peso. Letras maisculas possuem 15 pontos; minusculas possuem 25 pontos e Símbolos
        possuem 35 pontos. Todos esses valorres sao somados e arredondados(math.round). Cada criterio tem participação
        na definição da alrgura da barra visual de segurança
        */
    }

    function calculatefontSize() {
        if(passwordLength > 45) {
            inputEl.classList.remove("font-sm")
            inputEl.classList.remove("font-xs")
            inputEl.classList.add("font-xxs")

        } else if (passwordLength > 32 ) {
            inputEl.classList.remove("font-sm")
            inputEl.classList.add("font-xs")
            inputEl.classList.remove("font-xxs")

        }else if (passwordLength > 22) {
            inputEl.classList.add("font-sm")
            inputEl.classList.remove("font-xs")
            inputEl.classList.remove("font-xxs")

        } else {
            inputEl.classList.remove("font-sm")
            inputEl.classList.remove("font-xs")
            inputEl.classList.remove("font-xxs")

        }
    }
    /* essa funcao é responsavel por alterar o tamanho da fonte do input de acordo com o tamando do length.
    Isso ocorre devido ao uso do classList, de novo. Ele acessa as classes de font na qual cada uma corresponde 
    a um tamanho de font, com isso ele remeve e adciona de acordo com a situação, garantindo que cada uma ative de uma vez somado so
    */

    function copy() { // funcao para copiar para a area de transferecenccia o que esta no campo input
        navigator.clipboard.writeText(inputEl.value) 
        /* navigator.clipboard é objeto do navegador(api) que expoe operações da area de transferecenccia
        passamos a variavel inputEl.value como referencia pois ela é a variavel que esta atribuida
        a senha que esta no input, logo ela vai ser copiada pela nova funcao
         */
    }

    const passwordLengthEl = document.querySelector("#password-length")
    passwordLengthEl.addEventListener("input", function () {
        passwordLength = passwordLengthEl.value
        document.querySelector("#password-length-text").innerText = passwordLength // o span se atualiza ao deslizar o range
        generatePassword()
       
        
        
        /* 
        addEventListener é responsável por fazer o site reagir sempre que o usuario move o range
        o querySelector busca o elemento que tem o id como password-lenght e o prepara para um evento
        que vai ser usado como parâmetro o input que é o range e uma função que irá ser inicializada
        toda vez que o evento acontecer. O passwordLength.value vai ser o valor que o usuario escolheu
        no controle deslizante (4 a 64), com isso a variavel global passwordLength se atualiza de acordo
        com o que o usuario escolhe, mudando também assim o limite do for inicial, fazendo a senha aumentar
        */
    }) 
    upperCasecheckEl.addEventListener("click", generatePassword)
    NumbercheckEl.addEventListener("click", generatePassword)
    symbolcheckEl.addEventListener("click", generatePassword)
    // ao cickar nas controles os passwords novos sao preenchidos no input

    document.querySelector("#copiar1").addEventListener("click", copy)
    document.querySelector("#copiar2").addEventListener("click", copy)
    document.querySelector("#renew").addEventListener("click", generatePassword)
   
    /* aqui criamos uma variavel que ira receber como atributo o button pelo seu respectivo id(#)
    depois usamos o addEventListener para essa nova variavel que ira fazer um listener ser chamado para
    o event click do botão e chamando a funcao copy criada anteriormente quando o botao ser clicado. Tambem adciona
    a funcao de gerar nova senha ao clicar no simbolo do renew
    */

    generatePassword()
    
``` 
Este projeto foi desenvolvido com o objetivo de praticar:

- Manipulação do DOM 

- Eventos em JavaScript

- Boas práticas de organização de código

- Lógica de geração de strings aleatórias

- Experiência do usuário (UX)   

---
# Autor   

Pedro Lucas (pedrlc)  
Estudante de Engenharia de Software
GitHub: https://github.com/pedrlc   
Linkedin: www.linkedin.com/in/pedro-lucas-lopes-monteiro

