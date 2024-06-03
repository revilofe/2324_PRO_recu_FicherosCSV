## Generar un jar ejecutable de una aplicación Kotlin Compose

Crear un archivo JAR ejecutable para una aplicación de Kotlin Compose para escritorio implica varios pasos que dependen de la configuración de tu entorno de desarrollo y las herramientas que estés usando. Voy a asumir que estás utilizando Gradle, que es la herramienta de construcción recomendada para proyectos Kotlin. Aquí te doy una guía paso a paso:

### 1. Configuración del Proyecto

Para un proyecto Kotlin Compose para escritorio, necesitas tener el entorno adecuado. Aquí hay un ejemplo de cómo configurar tu `build.gradle.kts` (Kotlin DSL):

```kotlin
plugins {
    kotlin("jvm") version "1.6.10"
    id("org.jetbrains.compose") version "1.0.0"
}

repositories {
    mavenCentral()
    maven("https://maven.pkg.jetbrains.space/public/p/compose/dev")
}

dependencies {
    implementation(compose.desktop.currentOs)
}

kotlin {
    jvmToolchain {
        (this as JavaToolchainSpec).languageVersion.set(JavaLanguageVersion.of(11))
    }
}


compose.desktop {
    application {
        mainClass = "MainKt"

        nativeDistributions {
            targetFormats(TargetFormat.Dmg, TargetFormat.Msi, TargetFormat.Deb)
            packageName = "recu2324App"
            packageVersion = "1.0.0"
        }
    }
}
```

### 2. Clase Principal

Asegúrate de tener una clase principal que inicie tu aplicación Compose. Por ejemplo, `Main.kt` podría verse así:

```kotlin
import androidx.compose.desktop.ui.tooling.preview.Preview
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material.MaterialTheme
import androidx.compose.material.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.window.Window
import androidx.compose.ui.window.application

fun main() = application {
    Window(onCloseRequest = ::exitApplication) {
        App()
    }
}

@Preview
@Composable
fun App() {
    Box(modifier = Modifier.fillMaxSize()) {
        Text("Hola, Compose para Desktop!", style = MaterialTheme.typography.h1)
    }
}
```

### 3. Compilar el Proyecto

Para compilar el proyecto y crear un JAR, puedes utilizar el siguiente comando en la terminal:

```bash
./gradlew packageUberJarForCurrentOS
```

Este comando es parte del plugin de JetBrains Compose, que automáticamente crea un JAR que incluye todas las dependencias necesarias para ejecutar la aplicación.

Esta tarea también puedes lanzarla desde la IntelliJ IDEA
![](assets/daaca3e1.png)


### 4. Ejecutar el JAR

Una vez que el JAR se ha compilado, puedes encontrarlo en el directorio `build/compose/jars`. Puedes ejecutarlo con:

```bash
java -jar ./build/compose/jars/recu2324App-1.0.0.jar
```

o también:

```bash
kotlin ./build/compose/jars/recu2324App-1.0.0.jar
```

Reemplaza `recu2324App-1.0.0.jar con el nombre del archivo JAR generado por tu construcción, ya que es posible que el nombre contenga el sistema operativo y la arquitectura de la máquina, por ejemplo: ./recu2324App-linux-x64-1.0.0.jar

### Notas Adicionales

- Asegúrate de que todas las rutas y versiones en las configuraciones coincidan con tu entorno y requisitos específicos.
- La función `packageUberJarForCurrentOS` del plugin de Compose simplifica mucho la creación de un archivo JAR ejecutable, pero asegúrate de tener la versión más reciente del plugin para evitar problemas de compatibilidad.

## SCRIPT bash

Lo siguiente es un script de Bash que simplemente pase todos los argumentos que recibe al comando `kotlin` para ejecutar tu archivo JAR, sin modificar o manejar estos argumentos de ninguna manera específica.

### El script

Aquí tienes un ejemplo del script:

```bash
#!/bin/bash

# Este script ejecuta un archivo JAR de Kotlin pasando todos los argumentos recibidos.

# Path al archivo JAR que quieres ejecutar
JAR_PATH="./recu2324App-1.0.0.jar

# Ejecutamos el comando kotlin con el archivo JAR y todos los argumentos pasados al script
kotlin $JAR_PATH "$@"
```

### Instrucciones para usar el script:

1. **Guarda el script**: Crea un archivo con este script, por ejemplo, `recu2324App.sh`.
2. **Hazlo ejecutable**: Necesitas dar permisos de ejecución al script. Puedes hacerlo con el siguiente comando en la terminal:
   ```bash
   chmod +x recu2324App.sh
   ```
3. **Ejecuta el script**: Ahora puedes ejecutar tu script seguido de los argumentos que desees pasar al archivo JAR. Por ejemplo:
   ```bash
   ./recu2324App.sh arg1 arg2 arg3
   ```

## Para Windows


Para realizar pruebas en línea de comandos debéis realizar un cambio en el build.gradle.kts:

```
compose.desktop {
application {
mainClass = "MainKt"

        nativeDistributions {
            targetFormats(TargetFormat.Dmg, TargetFormat.Msi, TargetFormat.Deb)
            packageName = "recu2324App"
            packageVersion = "1.0.0"
        }
    }
}
```

De esta forma conseguimos que al compilar el proyecto se genere el fichero `recu2324App-windows-x64-1.0.0.jar` en la ruta `**.\\build\\compose\\jars`.

A continuación solo tenéis que crearos los siguientes ficheros por lotes que he creado para compilar y después ejecutar la aplicación desde línea de comandos.

```
:: recu2324App.bat

@echo off
REM Execute the Java application with parameters

REM Check if the Java executable is available
java -version >nul 2>&1
if not %errorlevel%==0 (
    echo Java is not installed or not configured in the system PATH.
    pause
    exit /b
)

REM Run the jar file with all parameters passed to this script
java -jar recu2324App.jar %*
if %errorlevel% neq 0 (
    echo The application exited with an error.
    pause
    exit /b
)
```

```
:: Compile.bat

@echo off
REM Compile the project and create an UberJar for the current OS
call ./gradlew packageUberJarForCurrentOS

REM Check if the compilation was successful
if not exist "./build/compose/jars/recu2324App-windows-x64-1.0.0.jar" (
    echo Compilation failed, jar file not found.
    pause
    exit /b
)

REM Move and rename the jar file to the working directory
move /Y build\compose\jars\recu2324App-windows-x64-1.0.0.jar .\recu2324App.jar
if %errorlevel% neq 0 (
    echo Failed to move and rename the jar file.
    pause
    exit /b
)

echo Successfully moved and renamed the jar file.
pause
```

Una vez tienes los ficheros `compile.bat` y `recu2324App.bat` podéis ir al directorio de trabajo del proyecto y ejecutar desde el símbolo del sistema la aplicación. Por ejemplo:

> recu2324App -i

Recordad que si lo hacéis desde Windows PowerShell debéis anteponer `.\\` al proceso por lotes para ejecutarlo o añadir la ruta del proyecto al PATH (aunque sea de manera temporal)... pero yo no me complicaría y lo ejecutaría desde el CMD.

