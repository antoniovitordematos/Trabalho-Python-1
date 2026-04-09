# Trabalho-Python-1
# Jogo simples de aventura em texto

# dados do jogador
nome = ""
vida = 100
inventario = []
local_atual = "Clareira Tranquila"

# mapa do jogo
mapa = {
    "Clareira Tranquila": {
        "descricao": "Você está em uma clareira. Tem caminho para o leste.",
        "leste": "Caminho da Floresta",
        "itens": ["poção"]
    },
    "Caminho da Floresta": {
        "descricao": "Uma trilha na floresta. Dá pra ir oeste ou norte.",
        "oeste": "Clareira Tranquila",
        "norte": "Caverna Sombria"
    },
    "Caverna Sombria": {
        "descricao": "Uma caverna escura. Tem algo brilhando aqui.",
        "sul": "Caminho da Floresta",
        "itens": ["amuleto"]
    },
    "Pântano Misterioso": {
        "descricao": "Um pântano estranho... parece perigoso.",
        "leste": "Clareira Tranquila",
        "evento": True
    }
}

# função para mostrar status
def mostrar_status():
    print("\n----- STATUS -----")
    print("Nome:", nome)
    print("Vida:", vida)
    
    if len(inventario) > 0:
        print("Inventário:", ", ".join(inventario))
    else:
        print("Inventário: vazio")
    print("------------------")


# início do jogo
nome = input("Digite seu nome: ")
print("Bem-vindo,", nome)

jogando = True

while jogando:
    mostrar_status()
    
    print("\nLocal:", local_atual)
    print(mapa[local_atual]["descricao"])
    
    # mostrar itens
    if "itens" in mapa[local_atual]:
        if len(mapa[local_atual]["itens"]) > 0:
            print("Itens no local:", ", ".join(mapa[local_atual]["itens"]))
    
    # evento de dano
    if "evento" in mapa[local_atual]:
        if mapa[local_atual]["evento"] == True:
            print("Um monstro apareceu!")
            vida = vida - 20
            print("Você perdeu 20 de vida!")
            mapa[local_atual]["evento"] = False
    
    # ações
    acao = input("\nO que fazer? (andar, pegar, usar, sair): ").lower()
    
    if acao == "andar":
        direcao = input("Digite a direção (norte, sul, leste, oeste): ").lower()
        
        if direcao in mapa[local_atual]:
            local_atual = mapa[local_atual][direcao]
        else:
            print("Não dá pra ir pra esse lado.")
    
    elif acao == "pegar":
        item = input("Qual item? ").lower()
        
        if "itens" in mapa[local_atual]:
            if item in mapa[local_atual]["itens"]:
                inventario.append(item)
                mapa[local_atual]["itens"].remove(item)
                print("Você pegou:", item)
            else:
                print("Item não está aqui.")
        else:
            print("Não tem itens aqui.")
    
    elif acao == "usar":
        item = input("Qual item? ").lower()
        
        if item in inventario:
            if item == "poção":
                vida = vida + 20
                inventario.remove(item)
                print("Você usou a poção e recuperou vida!")
            
            elif item == "amuleto":
                print("Você usou o amuleto... VOCÊ VENCEU!")
                jogando = False
        else:
            print("Você não tem esse item.")
    
    elif acao == "sair":
        print("Saindo do jogo...")
        jogando = False
    
    else:
        print("Comando inválido.")
    
    # condição de derrota
    if vida <= 0:
        print("Você morreu. Fim de jogo.")
        jogando = False
