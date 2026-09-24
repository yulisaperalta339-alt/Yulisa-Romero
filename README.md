<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>COSAPI S.A. - Sistema Digital SSOMA</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        cosapi: {
                            50: '#f0f9ff',
                            100: '#e0f2fe',
                            500: '#0284c7',
                            600: '#0284c7',
                            700: '#0369a1',
                            900: '#0c4a6e',
                        }
                    }
                }
            }
        }
    </script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body class="bg-slate-100 flex justify-center items-center min-h-screen p-0 sm:p-4 font-sans">

    <!-- CONTENEDOR ESTILO MÓVIL (APP VIEW) -->
    <div class="w-full sm:max-w-md bg-white sm:rounded-3xl shadow-2xl overflow-hidden flex flex-col h-screen sm:h-[850px] border border-slate-200">
        
        <!-- BARRA SUPERIOR DE ESTADO -->
        <div class="bg-cosapi-900 text-white px-6 py-3 flex justify-between items-center text-xs">
            <span class="font-semibold"><i class="fa-solid fa-shield-halved text-cosapi-100 mr-1"></i> COSAPI S.A. SSOMA</span>
            <span id="reloj">12:00</span>
        </div>

        <!-- HEADER PRINCIPAL -->
        <div class="bg-gradient-to-r from-cosapi-700 to-cosapi-500 text-white px-6 py-4 shadow-md">
            <div class="flex justify-between items-center">
                <div>
                    <h1 class="text-lg font-bold">Gestión de Homologación</h1>
                    <p class="text-xs text-cosapi-100" id="rol-actual-txt">Seleccione su perfil de acceso</p>
                </div>
                <button onclick="irHome()" class="bg-white/20 hover:bg-white/30 text-white px-3 py-1 rounded-full text-xs font-medium transition">
                    <i class="fa-solid fa-house"></i> Inicio
                </button>
            </div>
        </div>

        <!-- CONTENIDO DINÁMICO DE LA APLICACIÓN -->
        <div class="flex-1 overflow-y-auto p-4 bg-slate-50" id="app-container">
            
            <!-- VISTA 0: SELECCIÓN DE PERFIL (LOGIN INICIAL) -->
            <div id="view-login" class="space-y-6 my-auto pt-8">
                <div class="text-center space-y-2">
                    <div class="bg-cosapi-100 text-cosapi-700 w-16 h-16 rounded-full flex items-center justify-center mx-auto text-2xl shadow-inner">
                        <i class="fa-solid fa-hard-hat"></i>
                    </div>
                    <h2 class="text-xl font-bold text-slate-800">Sistema Digital SSOMA</h2>
                    <p class="text-xs text-slate-500 px-6">Flujo BPMN de Aprobación de Trabajadores y Homologación de Contratistas[cite: 1]</p>
                </div>

                <div class="space-y-3 pt-4">
                    <button onclick="cambiarVista('subcontratista')" class="w-full bg-white border-2 border-cosapi-500 text-cosapi-700 hover:bg-cosapi-50 p-4 rounded-xl font-semibold shadow-sm flex items-center justify-between transition">
                        <span class="flex items-center gap-3"><i class="fa-solid fa-user-plus text-lg text-cosapi-600"></i> Portal Subcontratista</span>
                        <i class="fa-solid fa-chevron-right text-xs"></i>
                    </button>
                    <button onclick="cambiarVista('revision')" class="w-full bg-cosapi-600 hover:bg-cosapi-700 text-white p-4 rounded-xl font-semibold shadow-md flex items-center justify-between transition">
                        <span class="flex items-center gap-3"><i class="fa-solid fa-clipboard-check text-lg"></i> Portal de Revisión (Admin / SSOMA)</span>
                        <i class="fa-solid fa-chevron-right text-xs"></i>
                    </button>
                </div>

                <div class="bg-cosapi-50 p-4 rounded-xl border border-cosapi-100 text-xs text-slate-600 space-y-1 mt-8">
                    <p class="font-bold text-cosapi-900"><i class="fa-solid fa-circle-info"></i> Nota Importante:</p>
                    <p>La aprobación final requiere la validación conjunta de Administración y Seguridad SSOMA de COSAPI S.A.[cite: 1]</p>
                </div>
            </div>

            <!-- VISTA 1: PORTAL SUBCONTRATISTA -->
            <div id="view-subcontratista" class="space-y-4 hidden">
                <div class="flex justify-between items-center mb-2">
                    <h3 class="font-bold text-slate-800 text-sm">Registro de Nuevo Trabajador</h3>
                    <span class="text-xs bg-cosapi-100 text-cosapi-700 px-2 py-0.5 rounded font-medium">Subcontrata</span>
                </div>

                <form id="form-trabajador" onsubmit="registrarTrabajador(event)" class="space-y-3 bg-white p-4 rounded-2xl shadow-sm border border-slate-200 text-xs">
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">Razón Social Subcontratista</label>
                        <input type="text" id="sub-empresa" value="Constructora Andina S.A.C." required class="w-full border rounded-lg p-2 bg-slate-50 text-slate-700">
                    </div>
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">Apellidos y Nombres</label>
                        <input type="text" id="sub-nombre" placeholder="Ej. Carlos Pérez Gómez" required class="w-full border rounded-lg p-2">
                    </div>
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">Cargo / Ocupación</label>
                        <select id="sub-cargo" class="w-full border rounded-lg p-2 bg-white">
                            <option>Operario Electricista</option>
                            <option>Oficial de Andamios</option>
                            <option>Operario Civil</option>
                            <option>Supervisor de Calidad</option>
                            <option>Rigging</option>
                        </select>
                    </div>

                    <div class="pt-2">
                        <label class="block font-bold text-slate-800 mb-2">📂 Carga del Checklist (8 Documentos PDF)[cite: 1]</label>
                        <div id="lista-inputs-docs" class="space-y-2">
                            <!-- Se generan por JS -->
                        </div>
                    </div>

                    <button type="submit" class="w-full bg-cosapi-600 hover:bg-cosapi-700 text-white font-bold py-3 rounded-xl shadow transition mt-4">
                        Enviar Expediente a Administración
                    </button>
                </form>

                <div class="mt-4">
                    <h4 class="font-bold text-slate-800 text-xs mb-2">Mis Expedientes Enviados:</h4>
                    <div id="lista-mis-solicitudes" class="space-y-2">
                        <!-- Renderizado dinámico -->
                    </div>
                </div>
            </div>

            <!-- VISTA 2: PORTAL DE REVISIÓN (ADMIN & SSOMA) -->
            <div id="view-revision" class="space-y-4 hidden">
                <!-- Pestañas internas de Revisión -->
                <div class="flex rounded-xl bg-slate-200 p-1 text-xs font-semibold">
                    <button onclick="cambiarTabRevision('admin')" id="tab-btn-admin" class="flex-1 py-2 rounded-lg bg-white text-cosapi-700 shadow-sm transition">1. Administración</button>
                    <button onclick="cambiarTabRevision('ssoma')" id="tab-btn-ssoma" class="flex-1 py-2 rounded-lg text-slate-600 transition">2. Seguridad SSOMA</button>
                    <button onclick="cambiarTabRevision('dash')" id="tab-btn-dash" class="flex-1 py-2 rounded-lg text-slate-600 transition">3. Dashboard</button>
                </div>

                <!-- SUBTAB 1: ADMINISTRACIÓN -->
                <div id="subtab-admin" class="space-y-3">
                    <h4 class="font-bold text-slate-800 text-xs">Bandeja de Revisión Documental y Legal</h4>
                    <div id="bandeja-admin" class="space-y-3">
                        <!-- Creado por JS -->
                    </div>
                </div>

                <!-- SUBTAB 2: SEGURIDAD SSOMA -->
                <div id="subtab-ssoma" class="space-y-3 hidden">
                    <h4 class="font-bold text-slate-800 text-xs">Bandeja Técnica y Aptitud Médica</h4>
                    <div id="bandeja-ssoma" class="space-y-3">
                        <!-- Creado por JS -->
                    </div>
                </div>

                <!-- SUBTAB 3: DASHBOARD Y CONSTANCIA -->
                <div id="subtab-dash" class="space-y-3 hidden">
                    <div class="grid grid-cols-2 gap-2">
                        <div class="bg-white p-3 rounded-xl border text-center shadow-sm">
                            <span class="text-xs text-slate-500">Total Solicitudes</span>
                            <h2 class="text-xl font-bold text-cosapi-700" id="kpi-total">0</h2>
                        </div>
                        <div class="bg-white p-3 rounded-xl border text-center shadow-sm">
                            <span class="text-xs text-slate-500">Habilitados Finales</span>
                            <h2 class="text-xl font-bold text-emerald-600" id="kpi-aprobados">0</h2>
                        </div>
                    </div>

                    <h4 class="font-bold text-slate-800 text-xs mt-4">Constancias de Aprobación Final Emitidas</h4>
                    <div id="bandeja-constancias" class="space-y-2">
                        <!-- Creado por JS -->
                    </div>
                </div>
            </div>

        </div>

        <!-- MODAL VISOR DE PDF -->
        <div id="modal-pdf" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center p-4 z-50">
            <div class="bg-white w-full max-w-sm rounded-2xl overflow-hidden shadow-2xl flex flex-col max-h-[80vh]">
                <div class="bg-cosapi-900 text-white px-4 py-3 flex justify-between items-center text-xs">
                    <span class="font-bold" id="modal-pdf-title">Visor de Documento PDF</span>
                    <button onclick="cerrarPdf()" class="text-white hover:text-red-300 font-bold px-2"><i class="fa-solid fa-xmark text-sm"></i></button>
                </div>
                <div class="p-6 flex-1 flex flex-col items-center justify-center bg-slate-50 text-center space-y-3">
                    <i class="fa-solid fa-file-pdf text-red-500 text-5xl"></i>
                    <p class="text-xs font-semibold text-slate-700" id="modal-file-name">documento.pdf</p>
                    <span class="text-[10px] bg-emerald-100 text-emerald-700 px-2 py-1 rounded font-medium">Archivo Validado digitalmente</span>
                    <div class="w-full bg-white p-3 rounded-xl border text-left text-[11px] text-slate-500 space-y-1">
                        <p><b>Tamaño:</b> 1.2 MB</p>
                        <p><b>Formato:</b> PDF / Firma Electrónica</p>
                        <p><b>Estado:</b> Legible y conforme</p>
                    </div>
                </div>
                <div class="p-3 bg-white border-t flex justify-end">
                    <button onclick="cerrarPdf()" class="bg-cosapi-600 text-white px-4 py-2 rounded-xl text-xs font-bold">Cerrar Visor</button>
                </div>
            </div>
        </div>

        <!-- BARRA DE NAVEGACIÓN INFERIOR (MÓVIL) -->
        <div class="bg-white border-t border-slate-200 py-2 px-6 flex justify-around items-center text-slate-500 text-xs">
            <button onclick="irHome()" class="flex flex-col items-center hover:text-cosapi-600 transition">
                <i class="fa-solid fa-house text-sm"></i>
                <span>Inicio</span>
            </button>
            <button onclick="cambiarVista('subcontratista')" class="flex flex-col items-center hover:text-cosapi-600 transition">
                <i class="fa-solid fa-file-arrow-up text-sm"></i>
                <span>Subir Docs</span>
            </button>
            <button onclick="cambiarVista('revision')" class="flex flex-col items-center hover:text-cosapi-600 transition">
                <i class="fa-solid fa-list-check text-sm"></i>
                <span>Revisión</span>
            </button>
        </div>

    </div>

    <!-- SCRIPT DE LÓGICA -->
    <script>
        // Reloj en tiempo real
        setInterval(() => {
            const now = new Date();
            document.getElementById('reloj').innerText = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
        }, 1000);

        // Lista de los 8 documentos requeridos del BPMN
        const listaDocsNombres = [
            "3.1 Alta T-Registro",
            "3.2 Antecedentes Penales y Policiales",
            "3.3 Carné RETCC",
            "3.4 Contrato de Trabajo",
            "3.5 CV y DNI Vigente",
            "3.6 SCTR Salud y Pensión",
            "3.7 Apto Médico",
            "3.8 Carné de Vacunación"
        ];

        // Base de datos en memoria
        let baseDatosTrabajadores = [
            {
                id: "REQ-2026-001",
                empresa: "Constructora Andina S.A.C.",
                nombre: "Carlos Pérez Gómez",
                cargo: "Operario Electricista",
                fecha: "24/09/2026",
                docs: listaDocsNombres.reduce((acc, doc) => { acc[doc] = { estado: "Aprobado", archivo: doc.toLowerCase().replace(/[^a-z0-9]/g, '_') + ".pdf" }; return acc; }, {}),
                estadoAdmin: "Aprobado",
                estadoSsoma: "Aprobado",
                induccion: true
            }
        ];

        // Inicializar inputs de documentos en el formulario
        const contenedorInputs = document.getElementById('lista-inputs-docs');
        listaDocsNombres.forEach(doc => {
            contenedorInputs.innerHTML += `
                <div class="flex items-center justify-between bg-slate-50 p-2 rounded-lg border text-[11px]">
                    <span class="font-medium text-slate-700 truncate w-36">${doc}</span>
                    <input type="file" accept=".pdf" class="text-[10px] text-slate-500 w-44 file:mr-2 file:py-1 file:px-2 file:rounded-md file:border-0 file:text-[10px] file:font-semibold file:bg-cosapi-100 file:text-cosapi-700 hover:file:bg-cosapi-200" required>
                </div>
            `;
        });

        // Cambio de Vistas
        function cambiarVista(vista) {
            document.getElementById('view-login').classList.add('hidden');
            document.getElementById('view-subcontratista').classList.add('hidden');
            document.getElementById('view-revision').classList.add('hidden');

            if(vista === 'login') {
                document.getElementById('view-login').classList.remove('hidden');
                document.getElementById('rol-actual-txt').innerText = "Seleccione su perfil de acceso";
            } else if(vista === 'subcontratista') {
                document.getElementById('view-subcontratista').classList.remove('hidden');
                document.getElementById('rol-actual-txt').innerText = "Portal Subcontratista - Subida de Documentos";
                renderMisSolicitudes();
            } else if(vista === 'revision') {
                document.getElementById('view-revision').classList.remove('hidden');
                document.getElementById('rol-actual-txt').innerText = "Portal de Revisión y Validación SSOMA";
                renderBandejasRevision();
            }
        }

        function irHome() {
            cambiarVista('login');
        }

        // Subtabs de Revisión
        function cambiarTabRevision(tab) {
            document.getElementById('subtab-admin').classList.add('hidden');
            document.getElementById('subtab-ssoma').classList.add('hidden');
            document.getElementById('subtab-dash').classList.add('hidden');
            
            document.getElementById('tab-btn-admin').className = "flex-1 py-2 rounded-lg text-slate-600 transition";
            document.getElementById('tab-btn-ssoma').className = "flex-1 py-2 rounded-lg text-slate-600 transition";
            document.getElementById('tab-btn-dash').className = "flex-1 py-2 rounded-lg text-slate-600 transition";

            if(tab === 'admin') {
                document.getElementById('subtab-admin').classList.remove('hidden');
                document.getElementById('tab-btn-admin').className = "flex-1 py-2 rounded-lg bg-white text-cosapi-700 shadow-sm transition";
            } else if(tab === 'ssoma') {
                document.getElementById('subtab-ssoma').classList.remove('hidden');
                document.getElementById('tab-btn-ssoma').className = "flex-1 py-2 rounded-lg bg-white text-cosapi-700 shadow-sm transition";
            } else if(tab === 'dash') {
                document.getElementById('subtab-dash').classList.remove('hidden');
                document.getElementById('tab-btn-dash').className = "flex-1 py-2 rounded-lg bg-white text-cosapi-700 shadow-sm transition";
                actualizarDashboard();
            }
        }

        // Registrar Trabajador
        function registrarTrabajador(e) {
            e.preventDefault();
            const empresa = document.getElementById('sub-empresa').value;
            const nombre = document.getElementById('sub-nombre').value;
            const cargo = document.getElementById('sub-cargo').value;

            const nuevoId = `REQ-2026-00${baseDatosTrabajadores.length + 1}`;
            
            let docsObj = {};
            listaDocsNombres.forEach(doc => {
                docsObj[doc] = { estado: "Pendiente", archivo: doc.toLowerCase().replace(/[^a-z0-9]/g, '_') + ".pdf" };
            });

            baseDatosTrabajadores.push({
                id: nuevoId,
                empresa: empresa,
                nombre: nombre,
                cargo: cargo,
                fecha: new Date().toLocaleDateString(),
                docs: docsObj,
                estadoAdmin: "En Revisión",
                estadoSsoma: "Sin revisar",
                induccion: false
            });

            alert(`¡Expediente ${nuevoId} enviado con éxito para revisión administrativa!`);
            document.getElementById('form-trabajador').reset();
            renderMisSolicitudes();
        }

        // Render Mis Solicitudes (Subcontratista)
        function renderMisSolicitudes() {
            const contenedor = document.getElementById('lista-mis-solicitudes');
            contenedor.innerHTML = "";
            baseDatosTrabajadores.forEach(t => {
                contenedor.innerHTML += `
                    <div class="bg-white p-3 rounded-xl border text-xs space-y-1 shadow-sm">
                        <div class="flex justify-between font-bold text-slate-800">
                            <span>${t.id} - ${t.nombre}</span>
                            <span class="text-cosapi-600">${t.cargo}</span>
                        </div>
                        <p class="text-slate-500 text-[11px]">Subcontrata: ${t.empresa}</p>
                        <div class="flex justify-between pt-1 text-[11px]">
                            <span>Admin: <b class="${t.estadoAdmin==='Aprobado'?'text-emerald-600':'text-amber-600'}">${t.estadoAdmin}</b></span>
                            <span>SSOMA: <b class="${t.estadoSsoma==='Aprobado'?'text-emerald-600':'text-amber-600'}">${t.estadoSsoma}</b></span>
                        </div>
                    </div>
                `;
            });
        }

        // Render Bandejas de Revisión
        function renderBandejasRevision() {
            const bandejaAdmin = document.getElementById('bandeja-admin');
            const bandejaSsoma = document.getElementById('bandeja-ssoma');
            
            bandejaAdmin.innerHTML = "";
            bandejaSsoma.innerHTML = "";

            baseDatosTrabajadores.forEach((t, idx) => {
                // HTML Admin
                let htmlDocs = "";
                for (let [docName, info] of Object.entries(t.docs)) {
                    htmlDocs += `
                        <div class="flex items-center justify-between bg-slate-50 p-1.5 rounded border text-[11px] mb-1">
                            <span class="truncate w-32 font-medium">${docName}</span>
                            <button onclick="abrirPdf('${info.archivo}', '${docName}')" class="text-cosapi-600 hover:underline font-bold text-[10px]"><i class="fa-solid fa-eye"></i> Ver PDF</button>
                            <select onchange="actualizarDocEstado(${idx}, '${docName}', this.value)" class="border rounded p-0.5 text-[10px] bg-white">
                                <option ${info.estado==='Pendiente'?'selected':''}>Pendiente</option>
                                <option ${info.estado==='Aprobado'?'selected':''}>Aprobado</option>
                                <option ${info.estado==='Observado'?'selected':''}>Observado</option>
                            </select>
                        </div>
                    `;
                }

                bandejaAdmin.innerHTML += `
                    <div class="bg-white p-3 rounded-xl border text-xs space-y-2 shadow-sm">
                        <div class="font-bold text-slate-800 flex justify-between">
                            <span>${t.id} - ${t.nombre}</span>
                            <span class="text-slate-500 text-[10px]">${t.fecha}</span>
                        </div>
                        <p class="text-slate-500 text-[11px]"><b>Cargo:</b> ${t.cargo} | <b>Empresa:</b> ${t.empresa}</p>
                        <div class="space-y-1 pt-1 border-t">
                            <p class="font-semibold text-slate-700 text-[11px]">Checklist de 8 Documentos:</p>
                            ${htmlDocs}
                        </div>
                        <div class="pt-2 flex items-center justify-between border-t">
                            <span class="font-bold text-[11px]">Dictamen Admin:</span>
                            <select onchange="baseDatosTrabajadores[${idx}].estadoAdmin = this.value" class="border rounded p-1 text-xs font-bold text-cosapi-700 bg-cosapi-50">
                                <option ${t.estadoAdmin==='En Revisión'?'selected':''}>En Revisión</option>
                                <option ${t.estadoAdmin==='Aprobado'?'selected':''}>Aprobado</option>
                                <option ${t.estadoAdmin==='Observado'?'selected':''}>Observado</option>
                            </select>
                        </div>
                    </div>
                `;

                // HTML SSOMA (Solo si Admin aprobó)
                if(t.estadoAdmin === "Aprobado") {
                    bandejaSsoma.innerHTML += `
                        <div class="bg-white p-3 rounded-xl border text-xs space-y-2 shadow-sm">
                            <div class="font-bold text-slate-800 flex justify-between">
                                <span>${t.id} - ${t.nombre}</span>
                                <span class="text-emerald-600 bg-emerald-50 px-2 py-0.5 rounded text-[10px]">Aprobado Admin</span>
                            </div>
                            <p class="text-slate-500 text-[11px]"><b>Subcontrata:</b> ${t.empresa}</p>
                            
                            <div class="bg-slate-50 p-2 rounded border space-y-2">
                                <label class="flex items-center gap-2 font-medium text-slate-700">
                                    <input type="checkbox" ${t.induccion?'checked':''} onchange="baseDatosTrabajadores[${idx}].induccion = this.checked" class="rounded text-cosapi-600">
                                    Inducción de Hombre Nuevo completada
                                </label>
                            </div>

                            <div class="pt-2 flex items-center justify-between border-t">
                                <span class="font-bold text-[11px]">Dictamen Técnico SSOMA:</span>
                                <select onchange="baseDatosTrabajadores[${idx}].estadoSsoma = this.value" class="border rounded p-1 text-xs font-bold text-cosapi-700 bg-cosapi-50">
                                    <option ${t.estadoSsoma==='Sin revisar'?'selected':''}>Sin revisar</option>
                                    <option ${t.estadoSsoma==='Aprobado'?'selected':''}>Aprobado</option>
                                    <option ${t.estadoSsoma==='Observado'?'selected':''}>Observado</option>
                                </select>
                            </div>
                        </div>
                    `;
                }
            });

            if(bandejaSsoma.innerHTML === "") {
                bandejaSsoma.innerHTML = `<p class="text-xs text-slate-400 text-center py-6">No hay expedientes habilitados por Administración todavía.</p>`;
            }
        }

        function actualizarDocEstado(idx, docName, val) {
            baseDatosTrabajadores[idx].docs[docName].estado = val;
        }

        // Actualizar Dashboard y Constancias
        function actualizarDashboard() {
            document.getElementById('kpi-total').innerText = baseDatosTrabajadores.length;
            let aprobados = baseDatosTrabajadores.filter(t => t.estadoAdmin === 'Aprobado' && t.estadoSsoma === 'Aprobado').length;
            document.getElementById('kpi-aprobados').innerText = aprobados;

            const contenedorConst = document.getElementById('bandeja-constancias');
            contenedorConst.innerHTML = "";

            baseDatosTrabajadores.forEach(t => {
                if(t.estadoAdmin === 'Aprobado' && t.estadoSsoma === 'Aprobado' && t.induccion) {
                    contenedorConst.innerHTML += `
                        <div class="bg-emerald-50 border border-emerald-200 p-3 rounded-xl text-xs space-y-2">
                            <div class="flex justify-between items-center">
                                <span class="font-bold text-emerald-900"><i class="fa-solid fa-circle-check text-emerald-600"></i> ${t.nombre}</span>
                                <span class="text-[10px] bg-emerald-200 text-emerald-800 px-2 py-0.5 rounded font-bold">HABILITADO</span>
                            </div>
                            <p class="text-slate-600 text-[11px]"><b>Cargo:</b> ${t.cargo} | <b>Empresa:</b> ${t.empresa}</p>
                            <button onclick="verConstancia('${t.id}', '${t.nombre}', '${t.cargo}', '${t.empresa}')" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white py-1.5 rounded-lg font-bold text-xs transition">
                                <i class="fa-solid fa-print"></i> Ver Constancia de Aprobación Final
                            </button>
                        </div>
                    `;
                }
            });

            if(contenedorConst.innerHTML === "") {
                contenedorConst.innerHTML = `<p class="text-xs text-slate-400 text-center py-4">Aún no hay trabajadores con aprobación conjunta (Admin + SSOMA + Inducción).</p>`;
            }
        }

        // Visor de PDF Modal
        function abrirPdf(archivo, titulo) {
            document.getElementById('modal-pdf-title').innerText = titulo;
            document.getElementById('modal-file-name').innerText = archivo;
            document.getElementById('modal-pdf').classList.remove('hidden');
        }

        function cerrarPdf() {
            document.getElementById('modal-pdf').classList.add('hidden');
        }

        // Ver Constancia en ventana emergente
        function verConstancia(id, nombre, cargo, empresa) {
            alert(`=== CONSTANCIA DE APROBACIÓN FINAL SSOMA ===\n\nN° Registro: ${id}\nTrabajador: ${nombre}\nCargo: ${cargo}\nSubcontratista: ${empresa}\n\n[ESTADO: HABILITADO PARA INGRESO A OBRA]\nAprobado por Administración y Seguridad SSOMA COSAPI S.A.[cite: 1]`);
        }
    </script>
</body>
</html>
