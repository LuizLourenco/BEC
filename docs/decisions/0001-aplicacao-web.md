# ADR-001: Aplicação Web (não desktop, não mobile nativo)

**Contexto:** o software precisa alcançar instituições com hardware variado (PCs antigos, tablets, celulares) e minimizar esforço de instalação/manutenção.

**Decisão:** construir uma aplicação **Web**, acessível via navegador, sem necessidade de instalação de software nativo.

**Alternativas consideradas:**
- App desktop (Electron, JavaFX): rejeitado — exige instalação/atualização manual em cada máquina, contraria o objetivo de baixo custo operacional e facilidade de implementação e uso para o público alvo.
- App mobile nativo (Android/iOS): rejeitado como abordagem principal — exigiria manter duas bases de código e a operação de biblioteca é predominantemente feita em computador.

**Consequências:** dependemos de boa performance em navegadores antigos/hardware modesto; reforça a necessidade de um frontend leve (ver ADR-004).

**Status:** decidido.
