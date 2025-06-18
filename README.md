<html lang="es">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Asistente de Configuración de Servidores (Corregido)</title>
        <script src="https://cdn.tailwindcss.com"></script>
        <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
        <style>
            body {
                font-family: 'Inter', sans-serif;
            }
            .code-block {
                background-color: #1E293B;
                color: #E2E8F0;
                padding: 1rem;
                border-radius: 0.5rem;
                font-family: 'monospace';
                white-space: pre-wrap;
                word-wrap: break-word;
                position: relative;
            }
            .code-block .copy-btn {
                position: absolute;
                top: 0.5rem;
                right: 0.5rem;
                background-color: #334155;
                color: #94A3B8;
                border: none;
                padding: 0.25rem 0.5rem;
                border-radius: 0.25rem;
                cursor: pointer;
                font-size: 0.8rem;
            }
            .code-block .copy-btn:hover {
                background-color: #475569;
            }
            .ip-placeholder, .user-placeholder {
                color: #34D399;
                font-weight: bold;
            }
            .path-placeholder {
                color: #FBBF24;
            }
            .explanation {
                background-color: #1F2937;
                border-left: 4px solid #38BDF8;
                padding: 1rem;
                margin-top: 0.5rem;
                border-radius: 0.25rem;
            }
            .nav-btn.active {
                background-color: #3B82F6;
                color: white;
            }
        </style>
    </head>
    <body class="bg-gray-900 text-gray-200">
    
        <div class="container mx-auto p-4 md:p-8">
            
            <header class="text-center mb-8">
                <h1 class="text-4xl font-bold text-white">Asistente de Configuración de Servidores</h1>
                <p class="text-gray-400 mt-2">Guía paso a paso para configurar Samba y DNS en Linux (Versión Corregida).</p>
            </header>
    
            <!-- Inputs -->
            <div class="mb-8 p-4 bg-gray-800 rounded-lg shadow-lg grid grid-cols-1 md:grid-cols-2 gap-4">
                <div>
                    <label for="ip_address" class="block text-lg font-medium text-white mb-2">Ingresa la IP del Servidor:</label>
                    <input type="text" id="ip_address" class="w-full p-3 bg-gray-700 border border-gray-600 rounded-md text-white focus:ring-2 focus:ring-blue-500" placeholder="Ej: 192.168.0.26">
                </div>
                <div>
                    <label for="username" class="block text-lg font-medium text-white mb-2">Ingresa tu Nombre de Usuario:</label>
                    <input type="text" id="username" class="w-full p-3 bg-gray-700 border border-gray-600 rounded-md text-white focus:ring-2 focus:ring-blue-500" placeholder="Ej: ciber">
                </div>
                 <p class="text-xs text-gray-500 mt-2 col-span-1 md:col-span-2">Los comandos y configuraciones se actualizarán automáticamente.</p>
            </div>
    
            <!-- Navegación -->
            <div class="flex justify-center mb-8 bg-gray-800 rounded-lg p-2">
                <button id="nav-samba" class="nav-btn active w-full md:w-auto text-center font-semibold py-2 px-6 rounded-md transition-colors duration-300">Servidor Samba</button>
                <button id="nav-dns" class="nav-btn w-full md:w-auto text-center font-semibold py-2 px-6 rounded-md transition-colors duration-300 ml-2">Servidor DNS</button>
            </div>
    
            <!-- Contenido Samba -->
            <div id="content-samba" class="space-y-6">
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 1: Actualización e Instalación</h3>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>sudo apt update && sudo apt upgrade -y
    sudo apt install samba -y</code></pre></div>
                    <div class="explanation"><p>Actualizamos los repositorios e instalamos Samba.</p></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 2: Crear Directorio a Compartir</h3>
                    <p class="mb-2">Crearemos la carpeta compartida dentro del home de tu usuario.</p>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>sudo mkdir -p <span class="path-placeholder">/home/ciber/samba/public</span></code></pre></div>
                    <div class="explanation"><p><strong><code>mkdir -p</code></strong>: Crea la estructura de directorios completa, incluso si los directorios padres no existen.</p></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 3: Establecer Permisos</h3>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>sudo chmod -R 0777 <span class="path-placeholder">/home/ciber/samba/public</span>
    sudo chown -R nobody:nogroup <span class="path-placeholder">/home/ciber/samba/public</span></code></pre></div>
                    <div class="explanation"><p>Se otorgan permisos amplios y se asigna la propiedad a <code>nobody:nogroup</code>. <strong>Nota:</strong> Aunque esto sigue el PDF, en un entorno de producción, es más seguro asignar la propiedad al usuario Samba (ej: <code>sudo chown -R ciber:sambashare ...</code>) para un control de permisos más estricto entre el sistema de archivos y Samba.</p></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 4: Crear y Habilitar Usuario Samba</h3>
                    <p class="mb-2">Samba mantiene su propia base de datos de usuarios. Debes añadir tu usuario del sistema a Samba y establecer una contraseña para él.</p>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>sudo smbpasswd -a <span class="user-placeholder">ciber</span>
    sudo smbpasswd -e <span class="user-placeholder">ciber</span></code></pre></div>
                    <div class="explanation"><p><strong><code>smbpasswd -a [usuario]</code></strong>: Añade un usuario a la base de datos de Samba. El usuario ya debe existir en el sistema. Te pedirá una contraseña.<br><strong><code>smbpasswd -e [usuario]</code></strong>: Habilita a un usuario de Samba previamente añadido.</p></div>
                </div>
                
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 5: Configurar Recurso Compartido</h3>
                    <p class="mb-2">Ejecuta <code>sudo nano /etc/samba/smb.conf</code> y añade esto al final del archivo. Esta configuración requiere autenticación.</p>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>[public]
       path = <span class="path-placeholder">/home/ciber/samba/public</span>
       browseable = yes
       writable = yes
       guest ok = no
       write list = <span class="user-placeholder">ciber</span>
       create mask = 0777
       directory mask = 0777</code></pre></div>
                    <div class="explanation"><p><strong><code>guest ok = no</code></strong>: Desactiva el acceso de invitados, forzando la autenticación.<br><strong><code>write list = [usuario]</code></strong>: Especifica la lista de usuarios que tienen permiso de escritura en este recurso.</p></div>
                </div>
                
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 6: Verificar y Reiniciar</h3>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>testparm
    sudo systemctl restart smbd nmbd</code></pre></div>
                    <div class="explanation"><p><strong><code>testparm</code></strong> valida el archivo de configuración. <strong><code>systemctl restart</code></strong> aplica los cambios reiniciando los servicios de Samba.</p></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 7: Permitir en Firewall</h3>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>sudo ufw allow samba
    sudo ufw status</code></pre></div>
                </div>
                
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 8: Acceder desde Windows</h3>
                    <p class="mb-2">En el Explorador de Archivos de Windows, ve a la barra de direcciones y escribe:</p>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>\\<span class="ip-placeholder">192.168.0.26</span></code></pre></div>
                    <div class="explanation"><p>Te pedirá credenciales. Usa el nombre de usuario y la contraseña que configuraste en el Paso 4.</p></div>
                </div>
            </div>
    
            <!-- Contenido DNS -->
            <div id="content-dns" class="hidden space-y-6">
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 1: Configurar IP Estática</h3>
                    <p class="mb-2">Un servidor DNS necesita una IP fija. Edita tu archivo de Netplan (<code>sudo nano /etc/netplan/00-installer-config.yaml</code> o similar).</p>
                    <div class="code-block" id="dns-step1-code"><button class="copy-btn">Copiar</button><pre><code>network:
      ethernets:
        enp0s3: # <-- Ojo: este nombre puede variar
          dhcp4: no
          addresses: [<span class="ip-placeholder">192.168.0.26</span>/24]
          gateway4: <span id="gateway-placeholder">192.168.0.1</span>
          nameservers:
            addresses: [8.8.8.8, 8.8.4.4]
      version: 2</code></pre></div>
                     <p class="mt-4">Aplica la configuración con: <code>sudo netplan apply</code></p>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 2: Instalar BIND9</h3>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>sudo apt update
    sudo apt install bind9 bind9-utils -y</code></pre></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 3: Configurar BIND para solo IPv4</h3>
                    <p class="mb-2">Editamos el archivo de configuración por defecto para que BIND escuche solo en IPv4, como se indica en el PDF.</p>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>sudo nano /etc/default/named</code></pre></div>
                    <p class="mt-2">Busca la línea <code>OPTIONS</code> y modifícala para que quede así:</p>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>OPTIONS="-u bind -4"</code></pre></div>
                     <div class="explanation"><p>La opción <strong><code>-4</code></strong> fuerza a BIND a usar únicamente el stack de red IPv4.</p></div>
                </div>
                
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 4: Configurar Opciones Globales</h3>
                    <p class="mb-2">Editamos <code>sudo nano /etc/bind/named.conf.options</code> para permitir consultas desde nuestra red local y definir los reenviadores (forwarders).</p>
                    <div class="code-block" id="dns-step4-code"><button class="copy-btn">Copiar</button><pre><code>acl "trusted" {
        localhost;
        <span id="network-placeholder">192.168.0.0/24</span>;
    };
    
    options {
        directory "/var/cache/bind";
        
        listen-on { any; };
        allow-query { trusted; };
        recursion yes;
    
        forwarders {
            8.8.8.8;
            8.8.4.4;
        };
    
        dnssec-validation no;
    };</code></pre></div>
                    <div class="explanation"><p>Hemos usado una ACL (Access Control List) llamada "trusted" para hacer la configuración más limpia. <strong><code>allow-query { trusted; };</code></strong> permite que solo las IPs en esa lista puedan hacer consultas.</p></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 5: Definir Zonas DNS</h3>
                    <p class="mb-2">En <code>sudo nano /etc/bind/named.conf.local</code>, definimos nuestra zona directa e inversa.</p>
                    <div class="code-block" id="dns-step5-code"><button class="copy-btn">Copiar</button><pre><code>zone "ciber.local" {
        type master;
        file "/etc/bind/zonas/db.ciber.local";
    };
    
    zone "<span id="reverse-zone-placeholder">0.168.192</span>.in-addr.arpa" {
        type master;
        file "/etc/bind/zonas/db.<span id="reverse-zone-file-placeholder">0.168.192</span>";
    };</code></pre></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 6: Crear Archivos de Zona</h3>
                    <p class="mb-2">Creamos el directorio y copiamos las plantillas, como indica el PDF, para evitar errores.</p>
                    <div class="code-block"><button class="copy-btn">Copiar</button><pre><code>sudo mkdir /etc/bind/zonas
    sudo cp /etc/bind/db.local /etc/bind/zonas/db.ciber.local</code></pre></div>
                    <p class="mt-4 mb-2">Ahora, edita el archivo de <strong>zona directa</strong>: <code>sudo nano /etc/bind/zonas/db.ciber.local</code></p>
                    <div class="code-block" id="dns-step6-forward-code"><button class="copy-btn">Copiar</button><pre><code>$TTL    604800
    @   IN  SOA servidor.ciber.local. root.ciber.local. (
                  2         ; Serial
             604800         ; Refresh
              86400         ; Retry
            2419200         ; Expire
             604800 )       ; Negative Cache TTL
    ;
    @            IN  NS      servidor.ciber.local.
    servidor     IN  A       <span class="ip-placeholder">192.168.0.26</span>
    equipo01     IN  A       <span id="other-ip-placeholder">192.168.0.27</span>
    </code></pre></div>
                    <p class="mt-4 mb-2">Copia el archivo que acabas de crear para usarlo como plantilla para la <strong>zona inversa</strong>.</p>
                    <div class="code-block" id="dns-step6-copy-reverse-code"><button class="copy-btn">Copiar</button><pre><code>sudo cp /etc/bind/zonas/db.ciber.local /etc/bind/zonas/db.<span id="reverse-zone-file-placeholder-2">0.168.192</span></code></pre></div>
                    <p class="mt-2 mb-2">Edita el nuevo archivo de <strong>zona inversa</strong>: <code>sudo nano /etc/bind/zonas/db.<span id="reverse-zone-file-placeholder-3">0.168.192</span></code></p>
                    <div class="code-block" id="dns-step6-reverse-code"><button class="copy-btn">Copiar</button><pre><code>$TTL    604800
    @   IN  SOA servidor.ciber.local. root.ciber.local. (
                  2         ; Serial
             604800         ; Refresh
              86400         ; Retry
            2419200         ; Expire
             604800 )       ; Negative Cache TTL
    ;
    @       IN  NS      servidor.ciber.local.
    <span id="reverse-ptr-placeholder">26</span>      IN  PTR     servidor.ciber.local.
    <span id="other-reverse-ptr-placeholder">27</span>      IN  PTR     equipo01.ciber.local.
    </code></pre></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 7: Verificar y Reiniciar BIND</h3>
                    <div class="code-block" id="dns-step7-code"><button class="copy-btn">Copiar</button><pre><code>sudo named-checkconf
    sudo named-checkzone ciber.local /etc/bind/zonas/db.ciber.local
    sudo named-checkzone <span id="reverse-zone-check-placeholder">0.168.192</span>.in-addr.arpa /etc/bind/zonas/db.<span id="reverse-zone-file-check-placeholder">0.168.192</span>
    sudo systemctl restart bind9</code></pre></div>
                    <div class="explanation"><p>Si los comandos <code>named-check...</code> no devuelven errores, la sintaxis es correcta.</p></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 8: Configurar Cliente DNS (Modo Persistente)</h3>
                    <p class="mb-2">Para evitar que <code>systemd-resolved</code> sobrescriba nuestra configuración, eliminaremos el enlace simbólico <code>/etc/resolv.conf</code>, crearemos un archivo estático y lo haremos inmutable.</p>
                    <div class="code-block" id="dns-step8-code"><button class="copy-btn">Copiar</button><pre><code>sudo rm /etc/resolv.conf
    sudo bash -c 'echo "nameserver <span class="ip-placeholder">192.168.0.26</span>" > /etc/resolv.conf'
    sudo bash -c 'echo "search ciber.local" >> /etc/resolv.conf'
    sudo chattr +i /etc/resolv.conf</code></pre></div>
                    <div class="explanation"><p><strong><code>rm</code></strong> elimina el symlink. <strong><code>bash -c 'echo ...'</code></strong> crea el nuevo archivo como root. <strong><code>chattr +i</code></strong> establece el atributo de inmutable, impidiendo que cualquier proceso (incluido el sistema) lo modifique.</p></div>
                </div>
    
                <div class="bg-gray-800 p-6 rounded-lg">
                    <h3 class="text-xl font-bold mb-4">Paso 9: Probar el Servidor DNS</h3>
                    <div class="code-block" id="dns-step9-code"><button class="copy-btn">Copiar</button><pre><code>dig servidor.ciber.local
    dig -x <span class="ip-placeholder">192.168.0.26</span></code></pre></div>
                </div>
            </div>
        </div>
    
        <script>
            const ipInput = document.getElementById('ip_address');
            const userInput = document.getElementById('username');
    
            function updateContent() {
                const ip = ipInput.value || '192.168.0.26';
                const user = userInput.value || 'ciber';
                const ipParts = ip.split('.');
                
                // Placeholders de IP
                document.querySelectorAll('.ip-placeholder').forEach(el => el.textContent = ip);
    
                // Placeholders de Usuario
                document.querySelectorAll('.user-placeholder').forEach(el => el.textContent = user);
    
                // Placeholders de Ruta Samba
                document.querySelectorAll('.path-placeholder').forEach(el => el.textContent = `/home/${user}/samba/public`);
                
                if (ipParts.length === 4) {
                    const gateway = ipParts.slice(0, 3).join('.') + '.1';
                    const network = ipParts.slice(0, 3).join('.') + '.0/24';
                    const reverseZone = ipParts.slice(0, 3).reverse().join('.');
                    const reverseZoneFile = ipParts.slice(0, 3).join('.');
                    const lastOctet = ipParts[3];
                    const otherIp = ipParts.slice(0, 3).join('.') + '.27';
                    
                    // DNS placeholders
                    document.getElementById('gateway-placeholder').textContent = gateway;
                    document.getElementById('network-placeholder').textContent = network;
                    document.getElementById('reverse-zone-placeholder').textContent = reverseZone;
                    document.getElementById('reverse-zone-file-placeholder').textContent = reverseZoneFile;
                    document.getElementById('reverse-zone-file-placeholder-2').textContent = reverseZoneFile;
                    document.getElementById('reverse-zone-file-placeholder-3').textContent = reverseZoneFile;
                    document.getElementById('reverse-ptr-placeholder').textContent = lastOctet;
                    document.getElementById('other-reverse-ptr-placeholder').textContent = '27'; // Assuming static
                    document.getElementById('reverse-zone-check-placeholder').textContent = reverseZone;
                    document.getElementById('reverse-zone-file-check-placeholder').textContent = reverseZoneFile;
                    document.getElementById('other-ip-placeholder').textContent = otherIp;
                }
            }
    
            ipInput.addEventListener('input', updateContent);
            userInput.addEventListener('input', updateContent);
    
            // Navegación
            const navSamba = document.getElementById('nav-samba');
            const navDns = document.getElementById('nav-dns');
            const contentSamba = document.getElementById('content-samba');
            const contentDns = document.getElementById('content-dns');
    
            navSamba.addEventListener('click', () => {
                contentSamba.classList.remove('hidden');
                contentDns.classList.add('hidden');
                navSamba.classList.add('active');
                navDns.classList.remove('active');
            });
    
            navDns.addEventListener('click', () => {
                contentDns.classList.remove('hidden');
                contentSamba.classList.add('hidden');
                navDns.classList.add('active');
                navSamba.classList.remove('active');
            });
    
            // Funcionalidad de copiar
            document.querySelectorAll('.copy-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const pre = e.target.nextElementSibling;
                    const code = pre.innerText;
                    navigator.clipboard.writeText(code).then(() => {
                        e.target.textContent = '¡Copiado!';
                        setTimeout(() => { e.target.textContent = 'Copiar'; }, 2000);
                    }).catch(err => {
                        console.error('Error al copiar: ', err);
                    });
                });
            });
    
            // Inicializar
            ipInput.value = '192.168.0.26';
            userInput.value = 'ciber';
            updateContent();
    
        </script>
    </body>
</html>
