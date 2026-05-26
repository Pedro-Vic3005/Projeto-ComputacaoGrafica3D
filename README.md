# 🧩 Cubo Mágico 3D

Simulação interativa de um Cubo Mágico (Rubik's Cube) construída com **Three.js** e JavaScript puro, rodando diretamente no navegador sem nenhuma instalação.

---

## 📋 Descrição

O projeto renderiza um cubo 3×3×3 com 27 cubinhos coloridos em uma cena 3D interativa. O jogador pode girar qualquer uma das 6 faces com animação suave, embaralhar o cubo automaticamente e tentar resolvê-lo — tudo no navegador, sem backend ou dependências externas além da biblioteca Three.js (carregada via CDN).

---

## ✨ Funcionalidades

- **Cubo 3×3×3** construído com 27 cubinhos individuais coloridos
- **6 faces rotacionáveis** (U, D, F, B, L, R) com botões dedicados
- **Animação suave** de 90° com interpolação de ângulo (18 frames por rotação)
- **Câmera orbital** — arraste o mouse para girar a perspectiva livremente
- **Embaralhar** com animação encadeada (20 movimentos aleatórios)
- **Contador de movimentos** em tempo real
- **Detecção de vitória** automática ao resolver o cubo
- **Reset** para restaurar o cubo ao estado inicial

---

## 🛠️ Tecnologias utilizadas

| Tecnologia | Versão | Finalidade |
|---|---|---|
| [Three.js](https://threejs.org/) | r128 | Motor de renderização 3D via WebGL |
| HTML5 | — | Estrutura da página e elemento `<canvas>` |
| CSS3 | — | Layout e estilização (tema escuro) |
| JavaScript (ES6+) | — | Lógica do cubo, animações e interações |

Nenhum framework adicional foi utilizado. O Three.js é carregado via CDN do `cdnjs.cloudflare.com`.

---

## 📁 Estrutura do projeto

```
cubo-magico-3d/
└── index.html        ← arquivo único com HTML, CSS e JavaScript
```

O projeto é um único arquivo HTML autocontido. Toda a lógica está encapsulada dentro de uma **IIFE** (Immediately Invoked Function Expression) no `<script>`, evitando poluição do escopo global.

---

## 🚀 Como executar

Não é necessário instalar nada. Basta abrir o arquivo no navegador:

```bash
# Clone o repositório
git clone https://github.com/SEU_USUARIO/cubo-magico-3d.git

# Entre na pasta
cd cubo-magico-3d

# Abra o arquivo no navegador (qualquer um dos comandos abaixo)
# Windows:
start index.html

# macOS:
open index.html

# Linux:
xdg-open index.html
```

> **Atenção:** é necessária conexão com a internet na primeira abertura para carregar o Three.js via CDN. Após isso, o projeto funciona offline.

---

## 🎮 Como usar

| Ação | Como fazer |
|---|---|
| Girar uma face | Clicar no botão correspondente (U / D / F / B / L / R) |
| Orbitar a câmera | Clicar e arrastar o mouse sobre o canvas |
| Embaralhar o cubo | Clicar no botão 🔀 Embaralhar |
| Reiniciar o cubo | Clicar no botão ↺ Reset |

### Notação das faces

```
U = Up    (face superior)    D = Down  (face inferior)
F = Front (face frontal)     B = Back  (face traseira)
L = Left  (face esquerda)    R = Right (face direita)
```

---

## 🏗️ Arquitetura do código

O JavaScript está organizado em 16 seções comentadas dentro do arquivo `index.html`:

### Seções principais

**1–4. Inicialização do Three.js**
Configura o `WebGLRenderer`, a `Scene`, a `PerspectiveCamera` e as luzes (`AmbientLight` + `DirectionalLight`). O canvas é dimensionado com `devicePixelRatio` para suporte a telas de alta resolução.

**5–6. Constantes e estado global**
Define a paleta de cores das 6 faces, o `GAP` (espaçamento entre cubinhos = `1.05`) e as variáveis de estado (`cubelets`, `moveCount`, `animating`).

**7–8. Construção do cubo (`makeCubelet` e `buildCube`)**
`makeCubelet(x, y, z)` cria um `THREE.Group` com um `THREE.Mesh` de 6 materiais independentes — um por face. A cor de cada face é determinada pela posição do cubinho na grade. `buildCube()` usa um loop triplo (`x`, `y`, `z` de −1 a 1) para gerar os 27 cubinhos.

**9. Seleção de face (`getFace`)**
Filtra os 9 cubinhos de uma determinada face usando a posição absoluta (`getWorldPosition`) dividida pelo `GAP` e arredondada para coordenada de grade.

**10–11. Rotação animada (`rotateFace`)**
A parte mais complexa do projeto. Funciona em 4 etapas:
1. Seleciona os 9 cubinhos da face com `getFace()`
2. Cria um `THREE.Group` temporário chamado **pivot** e move os cubinhos para dentro dele
3. Anima o pivot frame a frame com `requestAnimationFrame` até completar 90°
4. Aplica a transformação do pivot em cada cubinho via `applyMatrix4()`, devolve-os à cena e descarta o pivot

**12. Detecção de vitória (`checkVictory`)**
Para cada uma das 6 faces, coleta a cor do lado correspondente dos 9 cubinhos e verifica com `new Set(colors).size === 1` se todas são iguais.

**13–14. Botões e embaralhar**
Conecta os botões às chamadas de `rotateFace()`. O embaralhar encadeia 20 rotações aleatórias usando callbacks recursivos (padrão `next → next → next`).

**15–16. Câmera orbital e loop de renderização**
O arrasto do mouse calcula `dx`/`dy` e aplica um **Quaternion** de rotação à posição da câmera, que mantém o `lookAt(0,0,0)`. O `requestAnimationFrame` mantém o loop de renderização a ~60fps.

---

## 🔑 Conceitos-chave do Three.js utilizados

| Conceito | Uso no projeto |
|---|---|
| `THREE.Scene` | Palco 3D que contém todos os objetos |
| `THREE.PerspectiveCamera` | Ponto de vista com perspectiva realista |
| `THREE.WebGLRenderer` | Renderiza a cena na GPU via WebGL |
| `THREE.BoxGeometry` | Geometria cúbica de cada cubinho |
| `THREE.MeshLambertMaterial` | Material com reação à iluminação |
| `THREE.Group` | Container para agrupar e transformar objetos |
| `THREE.AmbientLight` | Luz difusa uniforme |
| `THREE.DirectionalLight` | Luz direcional (tipo sol) |
| `THREE.Quaternion` | Rotação 3D sem gimbal lock |
| `applyMatrix4()` | Congela transformação de um grupo em um objeto filho |
| `getWorldPosition()` | Posição absoluta na cena (ignora hierarquia) |
| `requestAnimationFrame` | Loop de animação nativo do navegador |

---

## 🔀 Histórico de commits

O projeto foi desenvolvido em **6 commits incrementais**, divididos entre 3 integrantes:

| Commit | Integrante | Descrição |
|---|---|---|
| `init: estrutura HTML e estilos CSS` | Pessoa 1 | Canvas, botões, placar e CSS |
| `feat: setup Three.js (cena, câmera, luzes e renderer)` | Pessoa 1 | Motor 3D inicializado |
| `feat: criação dos 27 cubinhos com cores por face` | Pessoa 2 | Cubo estático na tela |
| `feat: rotação de faces com animação e pivot` | Pessoa 2 | Botões de face funcionando |
| `feat: detecção de vitória, contador e embaralhar` | Pessoa 3 | Lógica de jogo completa |
| `feat: câmera orbital e loop de renderização` | Pessoa 3 | Interação final com mouse |

Para ver as diferenças entre commits:
```bash
git log --oneline
git show HASH_DO_COMMIT
```

---

## 👥 Integrantes

| Nome | Commits |
|---|---|
| Pessoa 1 | 1 e 2 |
| Pessoa 2 | 3 e 4 |
| Pessoa 3 | 5 e 6 |

---

## 📄 Licença

Projeto acadêmico desenvolvido para fins educacionais.