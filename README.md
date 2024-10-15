# Desafio Banco DIO



### Olá! Segue em anexo minha primeira tentativa de recriar um sistema bancário em Python pelo desafio da DIO.


```
Seja bem vindo!

[1] Depositar
[2] Sacar
[3] Extrato
[0] Sair

=> """

saldo = 0
limite = 500
extrato = ""
numero_saques = 0
LIMITE_SAQUES = 3

while True:

    opcao = input(menu)

    if opcao == "1":
        valor = float(input("Informe o valor que deseja depositar: ")) 

        if valor > 0:
           saldo += valor
           extrato += f"Depósito: R${valor:.2f}\n"
           print("\nDeposito no valor de R$", valor, "concluído com sucesso!")

        else:
            print("Operação falhou! O valor informado é inválido.")
        

    elif opcao == "2":
        valor = float(input("Informe o valor que deseja sacar: "))

        excedeu_saldo = valor > saldo

        excedeu_limite = valor > limite

        excedeu_saques = numero_saques >= LIMITE_SAQUES

        if excedeu_saldo:
            print("Operação falhou! Você não tem saldo suficiente.")

        elif excedeu_limite:
            print("Operação falhou! O valor informado excede o seu limite.")

        elif excedeu_saques:
            print("Operação falhou! Limite de saques diários excedido.")

        elif valor > 0:
            saldo -= valor
            extrato += f"Saque: R$ {valor:.2f}\n"
            numero_saques += 1
            print("\nSaque no valor de R$", valor, "concluído com sucesso!")

        else:
            print ("Operação falhou! O valor informado é inválido.")


    elif opcao == "3":
        print("\n===============EXTRATO===============")
        print("Não foram realizadas movimentações na conta." if not extrato else extrato)
        print(f"\nSaldo: R$ {saldo:.2f}")
        print("=======================================")
    
    elif opcao == "0":
        break

    else:
        print("Opção inválida, selecione novamente uma das operações disponíveis.")

```

### Segue também os ajustes visando uma otimização no sistema bancário.

```
import textwrap

def menu():
    menu = """\n
    ===================== MENU =====================
    [1]\tDepositar
    [2]\tSacar
    [3]\tExtrato
    [4]\tNovo usuário
    [5]\tNova conta
    [6]\tListar contatos
    [0]\tSair
    => """
    return input(textwrap.dedent(menu))
 
def depositar(saldo, valor, extrato, /):
    if valor > 0:
           saldo += valor
           extrato += f"Depósito: R${valor:.2f}\n"
           print("\n=== Deposito no valor de R$", valor, "concluído com sucesso! ===")

    else:
            print("\n@@@ Operação falhou! O valor informado é inválido. @@@")
            
    return saldo, extrato

def sacar(*, saldo, valor, extrato, limite, numero_saques, LIMITE_SAQUES):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= LIMITE_SAQUES 

    if excedeu_saldo:
            print("\n@@@ Operação falhou! Você não tem saldo suficiente. @@@")

    elif excedeu_limite:
            print("\n@@@ Operação falhou! O valor informado excede o seu limite. @@@")

    elif excedeu_saques:
            print("\n@@@ Operação falhou! Limite de saques diários excedido. @@@")
    
    elif valor > 0:
            saldo -= valor
            extrato += f"Saque: R$ {valor:.2f}\n"
            numero_saques += 1
            print("\n=== Saque no valor de R$", valor, "concluído com sucesso! ===")

    else:
            print ("@@@ Operação falhou! O valor informado é inválido. @@@")

    return saldo, extrato 

def exibir_extrato(saldo, /, *, extrato):
    print("\n===============EXTRATO===============")
    print("Não foram realizadas movimentações na conta." if not extrato else extrato)
    print(f"\nSaldo:\t\tR$ {saldo:.2f}")
    print("=======================================")

def criar_usuario(usuarios):
      cpf = input("Informe o CPF (somente número): ")
      usuario = filtrar_usuario(cpf, usuarios)

      if usuario:
            print("\n@@@ Já existe usuário com esse CPF! @@@")
            return
      
      nome = input("Informe o nome completo: ")
      data_nascimento = input("Informe a data de nascimento (dd-mm-aaaa): ")
      endereco = input("Infomre o seu endereço (logradouro, nro - bairro - cidade/sigla estado): ")

      usuarios.append ({"nome" : nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco})

      print("=== Usuário criado com sucesso! ===")

def filtrar_usuario(cpf, usuarios):
      usuarios_filtrados = [usuario for usuario in usuarios if usuario["cpf"] == cpf]
      return usuarios_filtrados[0] if usuarios_filtrados else None

def criar_conta(agencia, numero_conta, usuarios):
    cpf = input("Informe o CPF do usuário: ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
            print("\n=== Conta criada com sucesso! ===")
            return {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}

    print("\n@@@ Usuário não encontrado, fluxo de criação de conta encerrado! @@@")

def listar_contatos(contas):
    for conta in contas:
        linha = f"""
            Agência:\t{conta['agencia']}
            C/C:\t\t{conta['numero_conta']}
            Titular:\t{conta['usuario']['nome']}
        """
        print("=" * 100)
        print(textwrap.dedent(linha))

def main():
    LIMITE_SAQUES = 3
    AGENCIA = "0001"

    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0 
    usuarios = []
    contas = []

    while True:
        opcao = menu()

        if opcao == "1":
            valor = float(input("Informe o valor que deseja depositar: ")) 

            saldo, extrato = depositar(saldo, valor, extrato)

        elif opcao == "2":
            valor = float(input("Informe o valor que deseja sacar: "))

            saldo, extrato = sacar(
                  saldo=saldo,
                  valor=valor,
                  extrato=extrato,
                  limite=limite,
                  numero_saques=numero_saques,
                  LIMITE_SAQUES=LIMITE_SAQUES,
            )

        elif opcao == "3":
              exibir_extrato(saldo, extrato=extrato)

        elif opcao == "4":
              criar_usuario(usuarios)
        
        elif opcao == "5":
            numero_conta = len(contas) + 1
            conta = criar_conta(AGENCIA, numero_conta, usuarios)

            if conta:
                contas.append(conta)
        
        elif opcao == "6":
              listar_contatos(contas)

        elif opcao == "0":
            break

main()

``` 