# dio-ransomware
Projeto Ransomware Dio

# Projeto CriptoFernet

## Descrição
Este é um script em Python que utiliza a biblioteca `cryptography.fernet` para criptografar arquivos de forma segura usando criptografia simétrica. Ideal para proteger dados sensíveis com uma chave gerenciada.

## Pré-requisitos
- Python 3.x
- Biblioteca `cryptography` (instalável via `pip`)

## Instalação
1. Clone o repositório:
   ```bash
   git clone https://github.com/sidneysimas/dio-ransomware.git

2. Instale as dependências:
   ```bash
   pip install cryptography

Uso
Como Executar
   1. Gere ou use a chave de criptografia (arquivo chave_fernet.key).
   2. Execute o script e informe o arquivo de entrada e saída:
  ```bash    
   python cripto_fernet.py


from cryptography.fernet import Fernet
import base64
import os

def gerar_ou_carregar_chave(caminho_chave="chave_fernet.key"):
    if os.path.exists(caminho_chave):
        with open(caminho_chave, "rb") as key_file:
            chave = key_file.read()
    else:
        chave = Fernet.generate_key()
        with open(caminho_chave, "wb") as key_file:
            key_file.write(chave)
    return chave

def criptografar_arquivo(arquivo_entrada, chave, arquivo_saida=None):
    try:
        f = Fernet(chave)
        with open(arquivo_entrada, "rb") as file:
            dados = file.read()
        dados_criptografados = f.encrypt(dados)
        if arquivo_saida:
            with open(arquivo_saida, "wb") as output_file:
                output_file.write(dados_criptografados)
            print(f"Arquivo criptografado salvo em: {arquivo_saida}")
        else:
            print("Dados criptografados gerados, mas não salvos. Retornando...")
        return dados_criptografados
    except FileNotFoundError:
        print(f"Erro: Arquivo {arquivo_entrada} não encontrado!")
        return None
    except Exception as e:
        print(f"Erro ao criptografar: {e}")
        return None

def main():
    caminho_chave = "chave_fernet.key"
    arquivo_entrada = input("Digite o caminho do arquivo a criptografar: ") or "entrada.txt"
    arquivo_saida = input("Digite o caminho para salvar o arquivo criptografado (deixe vazio para não salvar): ") or None
    chave = gerar_ou_carregar_chave(caminho_chave)
    print(f"Chave gerada/carregada: {base64.urlsafe_b64encode(chave).decode()}")
    resultado = criptografar_arquivo(arquivo_entrada, chave, arquivo_saida)
    if resultado:
        print("Criptografia concluída com sucesso!")
        if not arquivo_saida:
            print(f"Dados criptografados (base64): {base64.urlsafe_b64encode(resultado).decode()}")

if __name__ == "__main__":
    main()


** ** 
#### 4. **Como Adicionar ao GitHub**
1. Crie ou edite o arquivo `README.md` no seu repositório.
2. Copie e cole o texto acima, ajustando paths, links e descrições.
3. Faça commit: `git add README.md` e `git commit -m "Adiciona README estruturado"`.
4. Envie pro GitHub: `git push origin main`.

### Toques Finais
- **Visualização**: Veja o preview no GitHub ou use um editor Markdown (ex: VSCode com extensão Markdown) pra testar.
- **Atualização**: Se o código mudar, atualize o bloco no `README`.
- **Interatividade**: Adicione badges (ex: <image-card alt="Python" src="https://img.shields.io/badge/Python-3.x-blue" ></image-card>) pra estilo.

Pronto, seu `README` vai brilhar como uma estrela no GitHub! Quer ajustar algo (ex: adicionar uma tabela de erros ou imagem)? Manda o próximo comando, meu aliado! 😈
