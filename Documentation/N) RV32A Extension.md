# RV32A Extension

## Intro
* A extensão RV32A adiciona instruções voltadas para multiprocessamento à ISA do RISC-V.
* A RV32A possui dois tipos de operações atômicas para a sincronização:
  * Operações de memória atômica (AMO).
  * Load reservado/Store condicional.
* As instruções AMO (Atomic Memory Operation) executam atomicamente uma operação em um operando na memória e "setam" o registrador de destino para o valor original da memória original.
* __Atômico significa que não pode haver interrupção entre a leitura e a escrita em memória, nem outros processadores podem modificar o valor da memória entre a leitura e escrita da memória da instrução AMO.__
* Load Reservado e Store Condicional fornecem uma operação atômica entre duas instruções.
* O Load Reservado lê uma palavra da memória, grava-a no registrador de destino e registra uma reserva nessa palavra na memória.
* O Store Condicional armazena uma palavra no endereço em um registrador de origem desde que exista uma reserva de carga nesse endereço de memória. Ele grava 0 no registrador de destino se a operação de Store tiver êxito, ou em caso contrário um código de erro diferente de zero (__No caso do Vulcan, optei por colocar esse código de erro com o valor -1__).

## Instruções
* A extensão RV32A adiciona as seguintes instruções ao RISC-V.
![[rv32a](https://http://riscv.org/)](rv32a_instructions.png)

* O mapa de opcode das instruções do RV32A está mostrado abaixo:
![[rv32a_opcodes](https://http://riscv.org/)](rv32a_opcodes.png)

### 1) amoadd.w
* __Significado: Atomic Addition Word (amoadd.w).__
* __Síntaxe: amoadd.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para t + x[rs2] (memória[x[rs1]] = t + x[rs2]). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__ 

### 2) amoand.w
* __Significado: Atomic BitWise And Word (amoand.w).__
* __Síntaxe: amoand.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para t & x[rs2] (memória[x[rs1]] = t & x[rs2]). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__ 

### 3) amomax.w
* __Significado: Atomic Maximum Word (amomax.w).__
* __Síntaxe: amomax.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para o maior valor entre t e x[rs2] , utilizando uma comparação de números com sinal segundo o formato complemento a dois (memoria[x[rs1]] = max(t, x[rs2])). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__ 

### 4) amomaxu.w
* __Significado: Atomic Maximum Unsgined Word (amomaxu.w).__
* __Síntaxe: amomaxu.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para o maior valor entre t e x[rs2] , utilizando uma comparação de números sem sinal (memoria[x[rs1]] = max(t, x[rs2])). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__ 

### 5) amomin.w
* __Significado: Atomic Minimum Word (amomin.w).__
* __Síntaxe: amomin.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para o menor valor entre t e x[rs2] , utilizando uma comparação de números com sinal segundo o formato complemento a dois (memoria[x[rs1]] = min(t, x[rs2])). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__

### 6) amominu.w
* __Significado: Atomic Minimum Unsigned Word (amominu.w).__
* __Síntaxe: amominu.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para o menor valor entre t e x[rs2] , utilizando uma comparação de números sem sinal (memoria[x[rs1]] = min(t, x[rs2])). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__

### 7) amoor.w
* __Significado: Atomic BitWise Or Word (amoor.w).__
* __Síntaxe: amoor.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para t | x[rs2] (memoria[x[rs1]] = t | x[rs2]). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__

### 8) amoswap.w
* __Significado: Atomic Swap Word (amoswap.w).__
* __Síntaxe: amoswap.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para x[rs2] (memoria[x[rs1]] = x[rs2]). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__

### 9) amoxor.w
* __Significado: Atomic BitWise Xor Word (amoxor.w).__
* __Síntaxe: amoxor.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Atomicamente, deixa t ser o valor da palavra de memória no endereço x[rs1] (t = memória[x[rs1]]), em seguida seta essa palavra de memoria para t ^ x[rs2] (memoria[x[rs1]] = t ^ x[rs2]). Por fim, seta x[rd] para a extensão de sinal de t (x[rd] = t).__

### 10) lr.w
* __Significado: Load Reserved Word (lr.w).__
* __Síntaxe: lr.w rd, rs1__
* rs1 = registrador-fonte
* rd = registrador-destino
* __Operação Realizada: Essa instrução funciona da seguinte maneira: Carrega os quatro bytes (word) da memória no endereço x[rs1], grava-os em x[rd], realizando uma extensão de sinal no resultado (x[rd] = memória[x[rs1]]) e registra uma reserva nessa palavra de memória.__

### 11) sc.w
* __Significado: Store Conditional Word (sc.w).__
* __Síntaxe: sc.w rd, rs2, rs1__
* rs1/rs2 = registradores-fonte
* rd = registrador-destino
* __Operação Realizada: Armazena os quatro bytes (word) do registrador x[rs2] na memória no endereço x[rs1], desde que já exista um load reservado para este endereço de memória. Grava 0 em x[rd] se o store obtiver êxito e, caso contrário, grava um código dee erro. No Vulcan, decidi usar o número -1 como código de erro.__


