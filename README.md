# Desafio Banco DIO



### Olá! Segue em anexo minha primeira tentativa de recriar o sistema bancário utilizado no desafio da DIO.


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

