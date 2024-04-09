def menu():

    menu = """\n
    ====SEJA BEM VINDO AO BANCO ROMANO====
    ================ MENU ================
    [1]\tDepositar
    [2]\tSacar
    [3]\tExtrato
    [4]\tSair
    => """
    return input(menu)


def depositar(saldo, valor, extrato, /):
    if valor > 0:
        saldo += valor
        extrato += f"Depósito:\tR$ {valor:.2f}\n"
        print("\n=== Depósito realizado com sucesso! ===")
    else:
        print("\n@@@ Operação falhou! O valor informado é inválido. @@@")

    return saldo, extrato

def sacar(*, saldo, valor, extrato, limite, numero_de_saques, limite_saques):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_de_saques >= limite_saques

    if excedeu_saldo:
        print("\nOperação falhou! Você não tem saldo suficiente para realizar o saque.\n")
        print("\nO Banco Romano agradece a preferência.\n")

    elif excedeu_limite:
        print("\nOperação falhou! O valor do saque excede o limite de R$: 500.00.\n")
        print("\nO Banco Romano agradece a preferência.\n")

    elif excedeu_saques:
        print("\nOperação falhou! Limite de 3 saques diários excedido.\n")
        print("\nO Banco Romano agradece a preferência.\n")

    elif valor > 0:
        saldo -= valor
        extrato += f"Saque:\t\tR$ {valor:.2f}\n"
        print(f"\nSaque no valor de R$: {valor:.2f} realizado com sucesso!")
        print("\nO Banco Romano agradece a preferência.\n")
        numero_de_saques += 1

    else:
        print("\nOperação falhou! O valor informado é inválido.")

    return saldo, extrato, numero_de_saques


def exibir_extrato(saldo, /, *, extrato):
    print("\n====SEJA BEM VINDO AO BANCO ROMANO====")
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo:\t\tR$ {saldo:.2f}")
    print("==========================================")

def main():
    LIMITE_SAQUES = 3
    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0

    while True:
        opcao = menu()

        if opcao == "1":
            valor = float(input("Informe o valor do depósito: "))

            saldo, extrato = depositar(saldo, valor, extrato)

        elif opcao == "2":
            valor = float(input("Informe o valor do saque: "))

            saldo, extrato, numero_saques = sacar(
                saldo=saldo,
                valor=valor,
                extrato=extrato,
                limite=limite,
                numero_de_saques=numero_saques,
                limite_saques=LIMITE_SAQUES,
            )

        elif opcao == "3":
            exibir_extrato(saldo, extrato=extrato)

        elif opcao == "4":
            break

        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")

main()
