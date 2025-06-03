Simulador de CPU em Pytho

Arquitetura e Organização de Computadores
Aluno: Ítalo Samuel Oliveira Silveira


# Registradores
registradores = {
    'R0': 0,
    'R1': 0,
    'R2': 0,
    'PC': 0
}

# Memória simulada
memoria = [0] * 256  # 256 posições de memória

# Carregar programa do arquivo
def carregar_programa(nome_arquivo):
    with open(nome_arquivo, 'r') as arquivo:
        return [linha.strip() for linha in arquivo if linha.strip()]

# Função para executar instruções
def executar_programa(programa):
    while registradores['PC'] < len(programa):
        instrucao = programa[registradores['PC']]
        print(f"> Executando: {instrucao}")  # Debug

        partes = instrucao.split()
        opcode = partes[0]

        if opcode == 'LOAD':
            reg = partes[1].strip(',')
            valor = int(partes[2])
            registradores[reg] = valor

        elif opcode == 'ADD':
            reg_dest = partes[1].strip(',')
            reg1 = partes[2].strip(',')
            reg2 = partes[3]
            registradores[reg_dest] = registradores[reg1] + registradores[reg2]

        elif opcode == 'STORE':
            reg = partes[1].strip(',')
            addr = int(partes[2])
            memoria[addr] = registradores[reg]

        elif opcode == 'HALT':
            print("Execução encerrada.")
            break

        else:
            print(f"Instrução desconhecida: {instrucao}")

        registradores['PC'] += 1

# Exibir estado da CPU e da memória
def exibir_estado():
    print("\nRegistradores:")
    for reg in ['R0', 'R1', 'R2', 'PC']:
        print(f"{reg} = {registradores[reg]}")
    print("\nMemória (endereços usados):")
    for i, val in enumerate(memoria):
        if val != 0:
            print(f"memória[{i}] = {val}")

# Executar tudo
if _name_ == '_main_':
    programa = carregar_programa('programa.txt')  # nome do seu arquivo de entrada
    executar_programa(programa)
    exibir_estado()
