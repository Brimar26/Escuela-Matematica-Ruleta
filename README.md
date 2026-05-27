
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>La Escuela - Ruleta Matemática Pro</title>
    <style>
        :root {
            --naranja: #FF9209;
            --azul: #40A2E3;
            --amarillo: #FFF67E;
            --verde-ok: #22C55E;
            --rojo-no: #EF4444;
            --blanco: #FFFBF5;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Arial Rounded MT Bold', sans-serif; }

        body {
            background-color: #F3F4F6;
            background-image: radial-gradient(var(--amarillo) 10%, transparent 11%), radial-gradient(var(--azul) 10%, transparent 11%);
            background-size: 30px 30px;
            background-position: 0 0, 15px 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .contenedor {
            background: var(--blanco);
            width: 100%;
            max-width: 780px;
            border-radius: 30px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.15);
            border: 5px solid var(--naranja);
            overflow: hidden;
            text-align: center;
        }

        .cabecera {
            background: var(--azul);
            color: white;
            padding: 15px;
            font-size: 24px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 30px;
            border-bottom: 5px solid var(--naranja);
        }

        .cronometro {
            background: rgba(0,0,0,0.2);
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 18px;
        }

        .panel-juego {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        /* --- Ruleta Grande Temática --- */
        .ruleta-contenedor {
            position: relative;
            margin: 20px 0;
            width: 340px;
            height: 340px;
        }

        .flecha {
            position: absolute;
            top: -18px;
            left: 50%;
            transform: translateX(-50%);
            width: 0;
            height: 0;
            border-left: 18px solid transparent;
            border-right: 18px solid transparent;
            border-top: 35px solid var(--rojo-no);
            z-index: 10;
        }

        .ruleta {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            border: 10px solid #333;
            background: conic-gradient(
                var(--azul) 0deg 90deg, 
                var(--verde-ok) 90deg 180deg, 
                var(--naranja) 180deg 270deg, 
                #FACC15 270deg 360deg
            );
            transition: transform 3.5s cubic-bezier(0.1, 0.8, 0.1, 1);
            position: relative;
            box-shadow: inset 0 0 25px rgba(0,0,0,0.35);
            overflow: hidden;
        }

        .ruleta-iconos {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            pointer-events: none;
        }

        .icono-sector {
            position: absolute;
            font-size: 26px;
            color: rgba(255, 255, 255, 0.95);
            text-shadow: 2px 2px 4px rgba(0,0,0,0.4);
        }

        .is-1 { top: 25%; left: 70%; transform: translate(-50%, -50%); }
        .is-2 { top: 70%; left: 70%; transform: translate(-50%, -50%); }
        .is-3 { top: 70%; left: 25%; transform: translate(-50%, -50%); }
        .is-4 { top: 25%; left: 25%; transform: translate(-50%, -50%); }

        .centro-ruleta {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 70px;
            height: 70px;
            background: white;
            border-radius: 50%;
            border: 5px solid #333;
            z-index: 5;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
        }

        .btn-girar {
            background: var(--naranja);
            color: white;
            border: none;
            padding: 12px 45px;
            font-size: 22px;
            font-weight: bold;
            border-radius: 15px;
            cursor: pointer;
            box-shadow: 0 5px 0 #C26D00;
            margin-bottom: 15px;
        }
        .btn-girar:active { transform: translateY(4px); box-shadow: none; }
        .btn-girar:disabled { background: #9CA3AF; box-shadow: none; cursor: not-allowed; }

        /* Contenedor de Preguntas */
        .seccion-pregunta {
            display: none;
            width: 100%;
            animation: fadeIn 0.4s ease-in-out;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .caja-operacion {
            background: #FEF3C7;
            border: 3px dashed var(--naranja);
            padding: 18px;
            font-size: 22px;
            border-radius: 20px;
            margin: 15px auto;
            max-width: 620px;
            line-height: 1.4;
            font-weight: bold;
            color: #333;
        }

        .zona-interactiva {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 40px;
            margin: 20px 0;
        }

        .avatar {
            font-size: 80px;
            transition: transform 0.3s;
            animation: flotar 2.5s ease-in-out infinite;
            background: #F3F4F6;
            width: 110px;
            height: 110px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 50%;
            border: 4px solid #E5E7EB;
        }
        @keyframes flotar { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }

        .bloque-opciones {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            width: 100%;
            max-width: 450px;
        }

        .opcion {
            padding: 18px;
            font-size: 24px;
            font-weight: bold;
            border-radius: 15px;
            border: 3px solid rgba(0,0,0,0.1);
            cursor: pointer;
            transition: background 0.2s, transform 0.1s;
            box-shadow: 0 4px 0 rgba(0,0,0,0.05);
        }
        .opcion:hover { transform: scale(1.03); }
        .opcion:nth-child(1) { background: #E0F2FE; color: #0369A1; border-color: var(--azul); }
        .opcion:nth-child(2) { background: #DCFCE7; color: #15803D; border-color: var(--verde-ok); }
        .opcion:nth-child(3) { background: #FFEDD5; color: #C2410C; border-color: var(--naranja); }
        .opcion:nth-child(4) { background: #FEF9C3; color: #A16207; border-color: #EAB308; }

        .feedback {
            font-size: 22px;
            font-weight: bold;
            margin-top: 15px;
            min-height: 30px;
        }

        /* Pantalla Final */
        .pantalla-final {
            display: none;
            padding: 40px 20px;
        }
        .score { font-size: 75px; color: var(--naranja); margin: 20px 0; }

        .btn-reiniciar {
            background: var(--azul);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 22px;
            border-radius: 15px;
            cursor: pointer;
            box-shadow: 0 5px 0 #1D4ED8;
        }
    </style>
</head>
<body>

    <div class="contenedor">
        <div class="cabecera">
            <span>🏫 Escuela Matemática - Ruleta</span>
            <div class="cronometro">⏱️ <span id="tiempo">0s</span></div>
        </div>

        <div id="juego-activo" class="panel-juego">
            <p style="font-size: 18px; color:#555; font-weight: bold;">Reto <span id="num-pregunta">1</span> de 10</p>
            
            <div class="ruleta-contenedor">
                <div class="flecha"></div>
                <div class="ruleta" id="ruleta">
                    <div class="ruleta-iconos">
                        <div class="icono-sector is-1">× 👦</div>
                        <div class="icono-sector is-2">÷ 📚</div>
                        <div class="icono-sector is-3">× 👧</div>
                        <div class="icono-sector is-4">÷ ✏️</div>
                    </div>
                    <div class="centro-ruleta">🎓</div>
                </div>
            </div>

            <button class="btn-girar" id="btn-girar" onclick="girarRuleta()">¡GIRAR! 🌀</button>

            <div class="seccion-pregunta" id="seccion-pregunta">
                <div class="caja-operacion" id="operacion-texto">¡Saliendo problema!</div>
                
                <div class="zona-interactiva">
                    <div class="avatar" id="avatar">😐</div>
                    <div class="bloque-opciones" id="opciones-contenedor"></div>
                </div>
                <div class="feedback" id="feedback-msg"></div>
            </div>
        </div>

        <div id="pantalla-final" class="pantalla-final">
            <h2>¡Reto Completado! 🏆</h2>
            <div class="score" id="porcentaje-final">0%</div>
            <p id="msg-desempeno" style="font-size: 22px; margin-bottom: 20px; font-weight: bold;"></p>
            <p id="tiempo-total" style="font-size: 18px; color: #666; margin-bottom: 30px;"></p>
            <button class="btn-reiniciar" onclick="reiniciarJuego()">Volver a Iniciar 🔄</button>
        </div>
    </div>

    <script>
        // Banco exacto de 10 preguntas escolares mezcladas aleatoriamente
        const bancoBase = [
            { operacion: "Un paquete trae 6 cuadernos. ¿Cuántos cuadernos hay en 8 paquetes para el grado?", correcta: 48, opciones: [42, 48, 54, 56] },
            { operacion: "La maestra tiene 35 lápices y los reparte equitativamente entre 5 mesas. ¿Cuántos recibe cada mesa?", correcta: 7, opciones: [6, 7, 8, 5] },
            { operacion: "En una caja vienen 9 tizas de colores. Si compramos 4 cajas, ¿cuántas tizas tenemos?", correcta: 36, opciones: [32, 36, 40, 28] },
            { operacion: "Se reparten 24 borradores entre 6 niños del equipo de limpieza. ¿Cuántos le tocan a cada uno?", correcta: 4, opciones: [3, 4, 5, 6] },
            { operacion: "Hay 7 filas de carpetas en el salón y cada fila tiene 6 carpetas. ¿Cuántas carpetas hay en total?", correcta: 42, opciones: [40, 48, 42, 36] },
            { operacion: "Para un juego se necesitan equipos de 4 niños. Si hay 32 niños en total, ¿cuántos equipos formarán?", correcta: 8, opciones: [7, 8, 9, 6] },
            { operacion: "Un block tiene 10 hojas de cartulina. Si la profesora compra 9 blocks, ¿cuántas hojas tiene?", correcta: 90, opciones: [80, 85, 90, 100] },
            { operacion: "Si acomodamos 18 libros en 3 estantes de la biblioteca por igual, ¿cuántos libros van en cada estante?", correcta: 6, opciones: [5, 6, 7, 8] },
            { operacion: "Cada estudiante recibe 2 manzanas de merienda. Si hay 9 estudiantes, ¿cuántas manzanas se entregaron?", correcta: 18, opciones: [16, 18, 20, 14] },
            { operacion: "El director compró 40 pelotas de fútbol y las dividió entre 8 secciones. ¿Cuántas pelotas recibe cada una?", correcta: 5, opciones: [4, 5, 6, 8] }
        ];

        let poolPreguntas = [];
        let preguntaActual = 1;
        const totalPreguntasPartida = 10;
        let respuestasCorrectasDirectas = 0; 
        let intentosFallidosPorPregunta = 0;
        let segundos = 0;
        let timerInterval;
        let ejercicioSeleccionado = null;
        let gradosActuales = 0;

        let audioCtx = null;
        function inicializarAudio() {
            if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        }

        function emitirSonido(tipo, frecuencia = null) {
            inicializarAudio();
            if (!audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.connect(gain); gain.connect(audioCtx.destination);

            if (tipo === 'click-ruleta') {
                osc.type = 'sine'; osc.frequency.setValueAtTime(frecuencia || 400, audioCtx.currentTime);
                gain.gain.setValueAtTime(0.15, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.05);
                osc.start(); osc.stop(audioCtx.currentTime + 0.05);
            } else if (tipo === 'bien') {
                osc.type = 'triangle'; osc.frequency.setValueAtTime(350, audioCtx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(700, audioCtx.currentTime + 0.3);
                gain.gain.setValueAtTime(0.25, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
                osc.start(); osc.stop(audioCtx.currentTime + 0.3);
            } else if (tipo === 'mal') {
                osc.type = 'sawtooth'; osc.frequency.setValueAtTime(180, audioCtx.currentTime);
                osc.frequency.linearRampToValueAtTime(90, audioCtx.currentTime + 0.4);
                gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
                osc.start(); osc.stop(audioCtx.currentTime + 0.4);
            }
        }

        function iniciarCronometro() {
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                segundos++;
                document.getElementById('tiempo').innerText = segundos + 's';
            }, 1000);
        }

        function ordenarTandaAleatoria() {
            poolPreguntas = [...bancoBase].sort(() => Math.random() - 0.5);
        }

        function girarRuleta() {
            inicializarAudio();
            document.getElementById('btn-girar').disabled = true;
            document.getElementById('seccion-pregunta').style.display = 'none';
            document.getElementById('feedback-msg').innerText = '';
            document.getElementById('avatar').innerText = '😐';
            intentosFallidosPorPregunta = 0;

            const vueltas = 4 + Math.floor(Math.random() * 3); 
            const gradosAdicionales = Math.floor(Math.random() * 360);
            gradosActuales += (vueltas * 360) + gradosAdicionales;
            
            document.getElementById('ruleta').style.transform = `rotate(${gradosActuales}deg)`;

            let tiempoDuracionTotal = 3500;
            let clicksTotales = 25;
            for (let i = 0; i < clicksTotales; i++) {
                let retraso = tiempoDuracionTotal * Math.pow(i / clicksTotales, 2);
                setTimeout(() => {
                    let tono = 350 + (clicksTotales - i) * 6;
                    emitirSonido('click-ruleta', tono);
                }, retraso);
            }

            setTimeout(() => {
                document.getElementById('btn-girar').disabled = false;
                desplegarPregunta();
            }, tiempoDuracionTotal);
        }

        function desplegarPregunta() {
            ejercicioSeleccionado = poolPreguntas[preguntaActual - 1];
            
            document.getElementById('operacion-texto').innerText = ejercicioSeleccionado.operacion;
            
            const opcionesMezcladas = [...ejercicioSeleccionado.opciones].sort(() => Math.random() - 0.5);
            const contenedor = document.getElementById('opciones-contenedor');
            contenedor.innerHTML = '';

            opcionesMezcladas.forEach(num => {
                const btn = document.createElement('div');
                btn.className = 'opcion';
                btn.innerText = num;
                btn.onclick = () => evaluarRespuesta(num, btn);
                contenedor.appendChild(btn);
            });

            document.getElementById('seccion-pregunta').style.display = 'block';
        }

        function evaluarRespuesta(seleccionado, elemento) {
            const avatar = document.getElementById('avatar');
            const feedback = document.getElementById('feedback-msg');

            if (seleccionado === ejercicioSeleccionado.correcta) {
                emitirSonido('bien');
                avatar.innerText = '😁'; 
                feedback.innerText = '¡Excelente! Respuesta Correcta 🎉';
                feedback.style.color = '#22C55E';
                
                if(intentosFallidosPorPregunta === 0) respuestasCorrectasDirectas++;

                document.querySelectorAll('.opcion').forEach(o => o.style.pointerEvents = 'none');
                
                setTimeout(() => {
                    if (preguntaActual < totalPreguntasPartida) {
                        preguntaActual++;
                        document.getElementById('num-pregunta').innerText = preguntaActual;
                        document.getElementById('seccion-pregunta').style.display = 'none';
                        feedback.innerText = '';
                        avatar.innerText = '😐';
                    } else {
                        finalizarJuego();
                    }
                }, 1800);

            } else {
                emitirSonido('mal');
                avatar.innerText = '😢'; 
                feedback.innerText = 'Respuesta incorrecta. ¡Vuelve a intentar! 💪';
                feedback.style.color = '#EF4444';
                intentosFallidosPorPregunta++;
                elemento.style.opacity = '0.4'; 
                elemento.style.pointerEvents = 'none';
            }
        }

        function finalizarJuego() {
            clearInterval(timerInterval);
            document.getElementById('juego-activo').style.display = 'none';
            document.getElementById('pantalla-final').style.display = 'block';

            const porcentaje = Math.round((respuestasCorrectasDirectas / totalPreguntasPartida) * 100);
            document.getElementById('porcentaje-final').innerText = porcentaje + "%";

            let mensaje = "";
            if (porcentaje === 100) mensaje = "¡Perfecto! Eres el rey de las matemáticas escolares 🏆🥇";
            else if (porcentaje >= 70) mensaje = "¡Muy buen puntaje! Tienes una agilidad genial ⚡";
            else mensaje = "¡Sigue practicando las operaciones! ¡Tú puedes mejorar! 📚";

            document.getElementById('msg-desempeno').innerText = mensaje;
            document.getElementById('tiempo-total').innerText = `Tiempo total de resolución: ${segundos} segundos.`;
        }

        function reiniciarJuego() {
            preguntaActual = 1;
            respuestasCorrectasDirectas = 0;
            segundos = 0;
            document.getElementById('num-pregunta').innerText = preguntaActual;
            document.getElementById('tiempo').innerText = '0s';
            document.getElementById('juego-activo').style.display = 'flex';
            document.getElementById('seccion-pregunta').style.display = 'none';
            document.getElementById('pantalla-final').style.display = 'none';
            document.getElementById('ruleta').style.transform = 'rotate(0deg)';
            gradosActuales = 0;
            ordenarTandaAleatoria();
            iniciarCronometro();
        }

        window.onload = () => {
            ordenarTandaAleatoria();
            iniciarCronometro();
        };
    </script>
</body>
</html>
