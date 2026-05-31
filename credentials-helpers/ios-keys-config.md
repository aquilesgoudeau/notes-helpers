//English

📘 iOS Production Credentials Manual (100% Local Workflow)

This document details how to set up a native, local iOS development environment to generate signed production .ipa files without relying on external cloud services (like EAS).

PART 1: One-Time Mac Configuration (First Time Only)

These components establish Apple's trust within your Mac's operating system and will serve all your future applications for as long as your developer account is active.

Step 1: Install and Update Xcode

To prevent conflicts with modern Node modules (like ExpoModulesCore), you need the latest native compilation engine.
Do not use the App Store if it experiences intermittent freezes. Download the official package directly from developer.apple.com/download/all/.
For Intel processors (like your 16-inch i9 MacBook Pro), download Xcode 26.5 Universal.xip.
Decompress the downloaded file by double-clicking it, then drag the Xcode icon into your Applications folder.
Open Xcode for the first time and allow it to complete downloading native system components (like the iOS Universal Simulator) until it reaches 100%.

Step 2: Install Apple's Intermediate Trust Certificate

This allows macOS to verify the authenticity of your distribution signatures without throwing red alert messages like "certificate is not trusted".
Download the official intermediate root bridge certificate: Apple Worldwide Developer Relations Certification Authority (G3).
Double-click the downloaded file (AppleWWDRCAG3.cer).
When the system asks where to store it, explicitly select the login keychain.

PART 2: Creating the Distribution Certificate (Valid for ALL your apps for 1 year)

The distribution certificate represents your digital production signature with Apple. A single certificate can be used to sign multiple independent applications until its expiration date.
Open Keychain Access on your Mac (Cmd + Space > type Keychain Access).
In the top screen menu bar, navigate to: Keychain Access > Certificate Assistant > Request a Certificate From a Certificate Authority...
In the form fields:
Enter your developer email address.
Enter your Common Name (e.g., Aquiles Leonardo).
Select "Saved to disk".
Click Continue and save the .certSigningRequest (CSR) file to your Desktop.
Log into the Apple Developer portal > Certificates, Identifiers & Profiles.
Go to Certificates, click the + button, and select Apple Distribution.
Upload the .certSigningRequest file you saved on your Desktop.
Apple will generate a .cer file. Download it.
Crucial Installation Step: Double-click the downloaded .cer file and save it strictly into the login keychain.
Visual Verification: Open Keychain Access, click the login keychain on the left sidebar, go to the My Certificates tab, and expand the gray arrow (>) to the left of your certificate. You must see your certificate with a green checkmark and its corresponding private key nested right below it in blue.

PART 3: Mandatory Workflow for EACH New Application (Applies to all future projects)

Every time you start a different Expo project (for example, to launch independent updates for a brand-new app), you must repeat these web and local steps to grant it its own distinct identity in the App Store.

Step A: Register the Unique App ID

Go to the Apple Developer portal > Certificates, Identifiers & Profiles > Identifiers.
Click the + button and select App IDs (type: App).
In Description, enter your app's commercial name (e.g., Money Savings).
In Bundle ID, select Explicit and input the exact string you defined in the ios.bundleIdentifier field of your local app.json file (e.g., com.leo1978.mobiletracker).
Click Continue and then Register.

Step B: Create the App Listing in App Store Connect (The Cloud Container)

While your local machine compiles, you can save time by setting up the store database.
Log into App Store Connect > My Apps.
Click the + button in the top-left corner and select New App.
Fill out the basic information:
Platform: iOS.
Name: Your app's commercial name.
Bundle ID: Select the exact App ID you registered in Step A from the dropdown menu (matching your app.json).
SKU: A unique internal tracking code of your choice (e.g., moneysavings-2026).
Click Create.

Step C: Generate the Provisioning Profile

The profile is the official web link that bundles your unique App ID with your active Distribution Certificate.
Return to the Apple Developer portal > Certificates, Identifiers & Profiles > Profiles.
Click the + button.
Under the Distribution section, select App Store (or Ad Hoc if it's for closed internal testing) and click Continue.
Select App ID: Choose the specific identifier created for this app in Step A.
Select Certificate: Choose your active Apple Distribution certificate (the one created in Part 2).
Give the profile a descriptive name (e.g., Money Savings App Store Profile) and click Generate.
Download the generated profile file and double-click it on your Mac to register it with the OS.

Step D: Final Sync in Xcode

Open the Xcode application on your Mac.
In the top screen menu bar, navigate to Xcode > Settings > Accounts.
Make sure you are signed in with your Apple ID Developer account.
Click on your account team name and hit the Download Manual Profiles button.
(This downloads and links the newly generated provisioning profile directly into your local compilers).
🚀 Ready to Build!
Once you complete these steps for your target app, open the terminal at the root of your Expo project and run your custom global command powered by the MCP protocol:
Bash
gemini.cli build-ios-ipa
What the automated prompt will safely handle behind the scenes:
It reads your local app.json to securely extract the bundleIdentifier and build/version numbers.
It queries your Mac's system security tools to locate your valid signing credentials from the login keychain.
It triggers npx expo prebuild --platform ios locally to generate the native code structures and install native CocoaPods dependencies.
It commands your new Xcode 26.5 CLI tools (xcodebuild) to compile a clean .xcarchive file in Release mode.
It autonomously generates a native ExportOptions.plist file to apply your local cryptographic signature and exports the final .ipa file package, ready to be uploaded straight to App Store Connect.



For your second application, the excellent news is that you don't have to do absolutely anything with your local keys! All the heavy cryptographic lifting is already taken care of on your Intel i9 MacBook Pro.
Here is exactly what happens with your keys and certificates for your next project:

1. What you DO NOT have to do again (Your current keys):

Do not repeat the Certificate Signing Request (CSR): You don't need to go back to the Certificate Assistant in Keychain Access.
Do not create another Distribution Certificate: The Apple Distribution certificate with its gray arrow and the private key that you showed me in the image Screenshot 2026-05-31 at 4.13.20 pm.png is for global use. It allows you to sign all the independent applications you want to create for an entire year.

2. The ONLY thing you DO have to do on the web (To give the new app its identity):

Since the certificate and the private key are already set up in your login keychain, for your second app you only need to follow the steps from Part 3 of our manual:
Register a new Identifier (App ID): Create a brand-new, unique Bundle ID in the Apple Developer portal (the same one you put in the app.json of your second project).
Create the app listing in App Store Connect: Set up the new web container for that specific app.
Generate a new Provisioning Profile: Create the profile on the web, linking the new App ID with the same distribution certificate you already have. Download it and double-click it.
Sync Xcode: Open Xcode, go to Settings > Accounts, and click on Download Manual Profiles so your machine downloads the new digital link.
In Summary:
Your local keys are universal for your account. For your second app, you simply register the new ID on the web, create its profile using your existing certificate, update Xcode with the manual download button, and that's it! Open your terminal in the new project and run gemini.cli build-ios-ipa.



//Spanish

📘 Manual de Configuración de Credenciales de Producción para iOS (Flujo 100% Local)

Este documento detalla cómo configurar un entorno de desarrollo iOS nativo y local sin depender de servicios externos en la nube (como EAS).

PARTE 1: Configuración Única en la Mac (Solo se hace la primera vez)

Estos componentes validan la confianza de Apple en el sistema operativo de tu Mac y te servirán para todas tus aplicaciones futuras durante la vigencia de tu cuenta.

Paso 1: Instalar y Actualizar Xcode

Para evitar conflictos con módulos de Node modernos (como ExpoModulesCore), necesitas el motor de compilación al día.
No uses la App Store si presenta bloqueos intermitentes. Descarga directamente el archivo oficial desde developer.apple.com/download/all/.
Para procesadores Intel (como tu i9 de 16 pulgadas), descarga Xcode 26.5 Universal.xip.
Descomprime el archivo haciendo doble clic y arrastra el icono de Xcode a tu carpeta de Aplicaciones.
Abre Xcode por primera vez y permite que complete la descarga de los componentes del sistema (como el iOS Universal Simulator) hasta que llegue al 100%.

Paso 2: Instalar el Certificado de Confianza Intermedio de Apple

Permite que macOS verifique la autenticidad de tus firmas de distribución sin arrojar alertas rojas de "certificate is not trusted".
Descarga el certificado raíz puente oficial: Apple Worldwide Developer Relations Certification Authority (G3).
Haz doble clic en el archivo descargado (AppleWWDRCAG3.cer).
Cuando el sistema te pregunte dónde guardarlo, selecciona obligatoriamente el llavero login (Inicio de sesión).

PARTE 2: Creación del Certificado de Distribución (Válido para TODAS tus apps por 1 año)

El certificado de distribución es tu "firma digital de producción" ante Apple. Un solo certificado te sirve para firmar múltiples aplicaciones independientes hasta que sople su fecha de vencimiento.
Abre Acceso a Llaveros (Keychain Access) desde tu Mac (Cmd + Espacio > Llaveros).
En la barra de menú superior de la pantalla, ve a: Acceso a Llaveros > Asistente de Certificados > Solicitar un Certificado de una Autoridad de Certificación...
En el formulario:
Escribe tu correo de desarrollador.
Escribe tu nombre común (ej. Aquiles Leonardo).
Selecciona "Se guarda en el disco".
Haz clic en Continuar y guarda el archivo .certSigningRequest (CSR) en tu Escritorio.
Entra al portal de Apple Developer > Certificates, Identifiers & Profiles.
Ve a Certificates, haz clic en el botón + y selecciona Apple Distribution.
Sube el archivo .certSigningRequest que creaste en tu Escritorio.
Apple generará un archivo .cer. Descárgalo.
Instalación Crucial: Haz doble clic en el archivo .cer descargado y guárdalo estrictamente en el llavero login.
Verificación visual: Abre Acceso a Llaveros, haz clic en el llavero login en la barra lateral, ve a la pestaña Mis Certificados y despliega la flecha gris (>) a la izquierda de tu certificado. Debes ver obligatoriamente tu certificado con un check verde y su correspondiente private key (clave privada) anidada abajo en azul.

PARTE 3: Flujo Obligatorio para CADA Nueva Aplicación (Aplica a proyectos futuros)

Cada vez que comiences un proyecto de Expo diferente (por ejemplo, para lanzar actualizaciones independientes de una nueva app), debes repetir estos pasos web y locales para asignarle una identidad propia en la App Store.

Paso A: Registrar el Identificador Único (App ID)

Entra al portal de Apple Developer > Certificates, Identifiers & Profiles > Identifiers.
Haz clic en el botón + y selecciona App IDs (tipo App).
En Description, pon el nombre de tu app (ej. Money Savings).
En Bundle ID, selecciona Explicit e ingresa la cadena de texto idéntica a la que definiste en el campo ios.bundleIdentifier de tu archivo app.json local (ej. com.leo1978.mobiletracker).
Haz clic en Continue y luego en Register.

Paso B: Crear la Ficha en App Store Connect (El contenedor en la nube)

Mientras compilas localmente, puedes ganar tiempo dejando armada la base de datos de la tienda.
Inicia sesión en App Store Connect > Mis Apps.
Haz clic en el botón + en la esquina superior izquierda y selecciona Nueva app.
Rellena los datos básicos:
Plataforma: iOS.
Nombre: Nombre comercial de tu app.
ID de lote (Bundle ID): Selecciona en el menú desplegable el App ID exacto que acabas de registrar en el Paso A (el de tu app.json).
SKU: Un código interno tuyo de seguimiento (ej. moneysavings-2026).
Haz clic en Crear.

Paso C: Generar el Perfil de Provisión (Provisioning Profile)

El perfil es el "vínculo" legal en la web que empaqueta tu App ID con tu Certificado de Distribución.
Regresa al portal de Apple Developer > Certificates, Identifiers & Profiles > Profiles.
Haz clic en el botón +.
Bajo la sección Distribution, selecciona App Store (o Ad Hoc si es para pruebas internas cerradas) y dale a Continuar.
Selecciona el App ID: Elige el identificador exclusivo de esta app específica (el creado en el Paso A).
Selecciona el Certificado: Elige tu certificado de Apple Distribution vigente (el que creamos en la Parte 2).
Ponle un nombre descriptivo al perfil (ej. Money Savings App Store Profile) y haz clic en Generate.
Descarga el archivo generado y hazle doble clic en tu Mac para registrarlo en el sistema.

Paso D: Sincronización final en Xcode

Abre la aplicación Xcode en tu Mac.
En la barra de menú superior, ve a Xcode > Settings > Accounts.
Asegúrate de tener iniciada tu sesión con tu Apple ID de Desarrollador.
Haz clic en tu cuenta y presiona el botón Download Manual Profiles.
(Esto descargará y enlazará el nuevo perfil que acabas de generar en la web directamente dentro de tus compiladores locales).
🚀 ¡Todo listo para compilar!
Una vez que completes estos pasos para una aplicación específica, abres la terminal en la raíz de tu proyecto de Expo y ejecutas tu comando personalizado basado en el protocolo MCP:
Bash
gemini.cli build-ios-ipa
¿Qué hará el prompt automatizado ahora de forma transparente?
Leerá tu app.json local para extraer de forma segura el bundleIdentifier y el número de versión.
Consultará las herramientas security del sistema de tu Mac buscando tu firma válida del llavero login.
Correrá npx expo prebuild --platform ios nativamente para inyectar las dependencias correspondientes de CocoaPods.
Usará las herramientas nativas de tu nuevo Xcode (xcodebuild) para compilar un archivo .xcarchive limpio en modo Release.
Creará de forma autónoma el archivo ExportOptions.plist para estampar tu firma digital local y exportará el archivo .ipa final empaquetado, directo para ser subido a App Store Connect.



¡Para tu segunda aplicación, la excelente noticia es que no tienes que hacer absolutamente nada con las llaves locales! Todo el trabajo pesado de criptografía ya quedó resuelto en tu MacBook Pro Intel i9.
Esto es exactamente lo que pasa con tus llaves y certificados para tu próximo proyecto:

1. Lo que NO tienes que volver a hacer (Tus llaves actuales):

No repitas la Solicitud de Certificado (CSR): No necesitas volver al Asistente de Certificados en el Acceso a Llaveros.
No crees otro Certificado de Distribución: El certificado Apple Distribution con su flecha gris y la private key (clave privada) que me mostraste en la imagen Screenshot 2026-05-31 at 4.13.20 pm.png es de uso global. Te sirve para firmar todas las aplicaciones independientes que quieras crear durante un año completo.

2. Lo único que SÍ tienes que hacer en la web (Para darle identidad a la nueva app):

Como el certificado y la llave privada ya están listos en tu llavero login, para tu segunda app solo debes seguir los pasos de la Parte 3 de nuestro manual:
Registrar un nuevo Identificador (App ID): Creas un nuevo Bundle ID exclusivo en el portal de desarrolladores de Apple (el mismo que le pongas al app.json de tu segundo proyecto).
Crear la ficha en App Store Connect: Armas el nuevo contenedor web para esa app en específico.
Generar un nuevo Perfil de Provisión (Provisioning Profile): Creas el perfil en la web vinculando el nuevo App ID con el mismo certificado de distribución que ya tienes. Lo descargas y le haces doble clic.
Sincronizar Xcode: Abres Xcode, vas a Settings > Accounts y haces clic en Download Manual Profiles para que tu máquina descargue el nuevo enlace digital.
En resumen:
Tus llaves locales son universales para tu cuenta. Para tu segunda app, simplemente registras el nuevo ID en la web, creas su perfil usando tu certificado existente, actualizas Xcode con el botón de descarga manual, ¡y listo! Abres tu terminal en el nuevo proyecto y ejecutas gemini.cli build-ios-ipa.