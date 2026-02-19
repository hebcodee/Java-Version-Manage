# Java Version Manage - JVM
`jvm` allows you to quickly use different versions of java via the command line.

**Example:**
```bash
$ jvm use 17
Now using java 17
$ java -version
java version "17.0.12" 2024-07-16 LTS
$ jvm use 21
Now using java 21
$ jvm use 25 -p
Now using java 25 /c/Program Files/Java/jdk-25

```
---
## Pre-requisitos:
- Ter instalado o Git Bash (`jvm` não possui suporte para cmd)
- Ter instalado a versao do java 17, 21 ou 25
  
## Como usar:

### Crie o arquivo ~/.jvm 
```
nano ~/.jvm
```

Cole isso:
```
#!/bin/bash

ACTION=$1
VERSION=$2
SHOW_PATH=$3

set_java() {
    JAVA_DIR="$1"

    # Git Bash (session only)
    export JAVA_HOME="$JAVA_DIR"
    export PATH="$JAVA_HOME/bin:$PATH"

    # Windows (permanently)
    cmd.exe /c "setx JAVA_HOME \"$JAVA_DIR\""
    cmd.exe /c "setx PATH \"$JAVA_DIR\\bin;%PATH%\""

    # Mensagem padrão
    echo "Now using java $VERSION"

    if [[ "$SHOW_PATH" == "-p" ]]; then
        echo "Path: $JAVA_HOME"
    fi
}

case "$ACTION" in
  use)
    case "$VERSION" in
      17)
        set_java "/c/Program Files/Java/jdk-17"
        ;;
      21)
        set_java "/c/Program Files/Java/jdk-21"
        ;;
      25)
        set_java "/c/Program Files/Java/jdk-25"
        ;;
      *)
        echo "Usage: jvm use {17|21|25} [-p]"
        exit 1
        ;;
    esac
    ;;
  *)
    echo "Usage: jvm use {17|21|25} [-p]"
    ;;
esac

```

### Tornar o script executável
```
chmod +x ~/.jvm
```

### Crie um alias permanente
```
echo "alias jvm='source ~/.jvm'" >> ~/.bashrc
```

Recarregar:
```
source ~/.bashrc
```

### 🎉 Agora funciona assim no Git Bash:
```
jvm use 21
```

Resultado:
```
Now using java 21
```

---

## Adicionando novas versoes:
### Abra o arquivo ~/.jvm 
```
nano ~/.jvm
```

### Edite o case adicionando uma nova versao
```
case "$ACTION" in
  use)
    case "$VERSION" in
      30)
        set_java "/c/Program Files/Java/jdk-30"
        ;;
      17)
        set_java "/c/Program Files/Java/jdk-17"
        ;;
      21)
        set_java "/c/Program Files/Java/jdk-21"
        ;;
      25)
        set_java "/c/Program Files/Java/jdk-25"
        ;;
      *)
        echo "Usage: jvm use {17|21|25} [-p]"
        exit 1
        ;;
    esac
    ;;
  *)
    echo "Usage: jvm use {17|21|25|30} [-p]"
    ;;
esac

```
