# ***Parallax!***
Ah, você sabe. Aquele efeito em três dimensôes. Não preciso deixar explícito a definição.
    
## ***Primeiro***, o "setup" da nossa página: `html`.
---
```html
     <!--- Class que define as propriedades de perspectiva e rolagem pela pagina inteira --->
    <div class="wrapper">
        <!--- Seções, dentro da divisão do efeito. O primeiro, que representa
        o background e seus elementos a frente dele; o segundo, 
        sem efeito — cortando a primeira seção — mostrando um específico conteúdo. --->
        
        <!--- O class="bg1" é usado para adicionar uma imagem como background. Veremos logo. --->
        <!--- O class="parallax" possuirá os códigos do efeito. --->
        <section class="section parallax bg1">
            <!--- O id é apenas para a estilização dos elementos. 
            
            Observe, que aqui, adicionei algumas divisões para colocar os elementos que me convim.
            Você consegue adicionar o que bem entender, e estiliza-lo como quiser.
            --->
           <div id="header">
                <input type="image" src="/imagens/logoamorzola.png" width="70px" height="70px" alt="logoet">
            </div>
            <div id=element-container>
                <a href="#"><input class="escutarbotao" type="button" value="ESCUTAR"></a>
                <a href="#"><input class="lojabotao" type="button" value="LOJA"></a>
            </div>
        </section>
         <!--- O class="static", também, apenas para estilização.
         Ele cortará a primeira seção enquanto você rolar (scroll) para baixo, ou para cima. 
         Ele possuirá um fundo (background-color). --->
        <section class="section static">
            <h1>Oxi, maluco</h1>
        </section>
    </div>
```
*Essa estrutura dentro do `html` se baseia em uma divisão inteira, com algumas seções dentro dela um abaixo da outra (em uma ordem). Uma seção, neste caso, é o background com o efeito parallax; e o outro, apenas uma seção comum.*<br>

O `wrapper` é uma das principais classes que faz com que o efeito funcione, pois é responsável pela página inteira até o fim da divisão. <br> As classes dessas `section` também fará com que o efeito funcione, então é importante. 
```html
<section class="section parallax bg1">
    &
<section class="section static">
```

## ***Segundo***, nossa estilização e processo do efeito:  `css`.
---

###### Reinicialização do `css`, antes de tudo. Hábito meu. 
```css
    * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    text-decoration: none;
}
```

_Configurações exatas do que não pode faltar nessa classe `wrapper`_. (`height`, `overflow`, `perspective`)
```css
/* 
  A altura (height) está em 100% do viewport (tela que está sendo vista no momento/tela-cheia).
  
  O transbordo/barra de rolagem (overflow) pode ser ativado horizontalmente devido ao tamanho da imagem. 
  Use "hidden" no eixo X (overflow-x: hidden;) para desativá-lo.

  A perspectiva (perspective) define a distância entre o usuário e o eixo-Z. Ponto-chave para o efeito, também.
  Quanto maior o valor, maior a distância entre você e o background, 
  onde consequentemente altera a escala dele.

*/
.wrapper {
    height: 100vh;
    overflow-x: hidden;
    overflow-y: auto;
    perspective: 3px;
}
```
_Aqui, estilizamos nossa primeira seção (`section`) abaixo do `wrapper`._
```css
.section {
    /* Uso uma borda (border) para ter uma noção da posição dos meus elementos. 
    Quando tudo acabar, eu apago. Faço isso várias vezes. */
    border: 1px solid red;
    
    /* Para que os elementos-filhos possam ser absolutamente posicionados 
    em relação ao elemento-pai. Aqui é importante.*/
    position: relative;
    height: 100vh;
    /* Obrigatório. A magia acontece aqui. */
    transform-style: preserve-3d;
}

/* 
---MEUS ELEMENTOS---
Estilize seus elementos abaixo da section. Esses são os meus.

#header {
    border: 1px solid red;
    display: inline-block;
    flex: 0 1 100%;
}
#element-container {
    border: 1px solid red;
    height: 80vh;
    display: flex;
    justify-content: space-around;
    align-items: flex-end;
    flex: 0 1 100%;
}
.escutarbotao, .lojabotao {
    font-size: 30px;
    font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
    background-color: transparent;
    color: white;
    border: transparent;
    
    padding: 10px;
    transition: 0.2s all;
}
.escutarbotao:hover, .lojabotao:hover {
    background-color: white;
    color: black;
} 
*/
```
O `transform-style: preserve-3d` faz parte, também, do segredo para o efeito funcionar. Ele indica que os filhos do elemento devem ser posicionados no espaço 3D. <br>
***

_Onde o efeito é formulado: `parallax`_.
```css
/* A classe adiciona um pseudo-elemento (::after) à imagem do background,
fornecendo as transformações necessárias para o efeito de paralaxe. */
.parallax::after {
    content: " ";
    /* Configuração do background. */
    background-attachment: fixed;
    background-size: cover;
    background-position: center center;
    /* Configuração absoluta para o efeito. */
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    /* A velocidade do parallax altera a escala (scale()). Deixei o valor um pouco maior 
    que -1px para dar mais efeito, mas como consequência, alterei a escala de novo. */
    transform: translateZ(-3px) scale(2.2);
    /* Altera as camadas. Nesse caso, so temos duas camadas (elementos e background). 
    Deixe o background em uma camada abaixo, como eu fiz aqui.*/
    z-index: -1;
}
```
Um ***pseudo-elemento*** é uma palavra-chave adicionada a um seletor que permite que você estilize uma parte específica do elemento selecionado. <br>
***
_Nossa segunda seção (`section`), abaixo do `parallax`._
```css
/* Coisa simples. Faça o que bem entender. */
.static {
    background: red;
}
/* Novamente, um pseudo-elemento para adição da imagem que queremos para o background. */
.bg1::after {
    background-image: url(/imagens/ETATUALIZADO.jpg);
}
```
