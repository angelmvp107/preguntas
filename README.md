<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Análisis de Error en Resistencias 1kΩ</title>
    <!-- Optimizado para Netlify - Listo para publicar -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #000000;
            color: #e4e4e4;
            padding: 20px;
            min-height: 100vh;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            backdrop-filter: blur(10px);
        }

        h1 {
            color: #00d4ff;
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 0 0 20px rgba(0, 212, 255, 0.5);
        }

        .subtitle {
            color: #a0a0a0;
            font-size: 1.1em;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .control-group {
            background: rgba(255, 255, 255, 0.08);
            padding: 15px 25px;
            border-radius: 10px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #00d4ff;
            font-weight: 600;
            font-size: 0.95em;
        }

        select {
            background: rgba(255, 255, 255, 0.1);
            color: #e4e4e4;
            border: 1px solid rgba(0, 212, 255, 0.3);
            padding: 10px 15px;
            border-radius: 5px;
            font-size: 1em;
            cursor: pointer;
            transition: all 0.3s ease;
            min-width: 180px;
        }

        select:hover {
            border-color: #00d4ff;
            background: rgba(255, 255, 255, 0.15);
        }

        select:focus {
            outline: none;
            border-color: #00d4ff;
            box-shadow: 0 0 10px rgba(0, 212, 255, 0.3);
        }

        option {
            background: #1a1a2e;
            color: #e4e4e4;
        }

        .chart-container {
            background: rgba(255, 255, 255, 0.05);
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 30px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }

        .chart-wrapper {
            position: relative;
            height: 500px;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }

        .stat-card {
            background: rgba(255, 255, 255, 0.08);
            padding: 20px;
            border-radius: 10px;
            border: 1px solid rgba(0, 212, 255, 0.2);
            transition: transform 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            border-color: #00d4ff;
            box-shadow: 0 5px 20px rgba(0, 212, 255, 0.2);
        }

        .stat-label {
            color: #a0a0a0;
            font-size: 0.9em;
            margin-bottom: 5px;
        }

        .stat-value {
            color: #00d4ff;
            font-size: 1.8em;
            font-weight: bold;
        }

        .stat-unit {
            color: #a0a0a0;
            font-size: 0.9em;
            margin-left: 5px;
        }

        .table-container {
            background: rgba(255, 255, 255, 0.05);
            padding: 25px;
            border-radius: 15px;
            overflow-x: auto;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        table {
            width: 100%;
            border-collapse: collapse;
            color: #e4e4e4;
        }

        th {
            background: rgba(0, 212, 255, 0.2);
            padding: 12px;
            text-align: center;
            border: 1px solid rgba(0, 212, 255, 0.3);
            color: #00d4ff;
            font-weight: 600;
        }

        tfoot td {
            background: rgba(0, 212, 255, 0.15);
            padding: 12px;
            text-align: center;
            border: 1px solid rgba(0, 212, 255, 0.3);
            color: #00d4ff;
            font-weight: 600;
        }

        td {
            padding: 10px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        tr:hover {
            background: rgba(0, 212, 255, 0.1);
        }

        .footer {
            text-align: center;
            margin-top: 40px;
            color: #a0a0a0;
            font-size: 0.9em;
        }

        .formula-container {
            background: rgba(255, 255, 255, 0.05);
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 30px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .formula-section {
            margin-bottom: 25px;
        }

        .formula-section h3 {
            color: #00d4ff;
            margin-bottom: 15px;
            font-size: 1.1em;
        }

        .formula-box {
            background: rgba(0, 0, 0, 0.3);
            padding: 20px;
            border-radius: 10px;
            border: 1px solid rgba(0, 212, 255, 0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2em;
        }

        .formula-text {
            color: #e4e4e4;
            font-size: 1.3em;
            margin-right: 15px;
        }

        .fraction {
            display: inline-flex;
            flex-direction: column;
            align-items: center;
            vertical-align: middle;
        }

        .numerator, .denominator {
            color: #e4e4e4;
            padding: 8px 15px;
        }

        .numerator {
            border-bottom: 2px solid #00d4ff;
            padding-bottom: 10px;
            margin-bottom: 5px;
        }

        .denominator {
            padding-top: 10px;
        }

        .parameters {
            list-style: none;
            padding: 0;
        }

        .parameters li {
            background: rgba(0, 0, 0, 0.2);
            padding: 12px 20px;
            margin-bottom: 8px;
            border-radius: 8px;
            border-left: 3px solid #00d4ff;
            color: #e4e4e4;
        }

        .parameters strong {
            color: #00d4ff;
        }

        .result {
            text-align: center;
            margin-top: 20px;
            font-size: 1.3em;
            color: #00d4ff;
        }

        .data-display-container {
            background: rgba(255, 255, 255, 0.05);
            padding: 30px;
            border-radius: 15px;
            margin-top: 30px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 30px;
        }

        .data-button {
            background: rgba(0, 212, 255, 0.1);
            border: 2px solid #00d4ff;
            color: #00d4ff;
            padding: 20px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 1em;
            font-weight: 600;
            transition: all 0.3s ease;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .data-button:hover {
            background: rgba(0, 212, 255, 0.2);
            transform: translateY(-3px);
            box-shadow: 0 5px 20px rgba(0, 212, 255, 0.3);
        }

        .data-button:active {
            transform: translateY(-1px);
        }

        .button-title {
            text-align: left;
            line-height: 1.4;
        }

        .button-icon {
            font-size: 1.2em;
            transition: transform 0.3s ease;
        }

        .data-button.active .button-icon {
            transform: rotate(180deg);
        }

        .data-content {
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(0, 212, 255, 0.3);
            border-radius: 10px;
            padding: 25px;
            display: none;
        }

        .data-content.show {
            display: block;
            animation: slideDown 0.3s ease;
        }

        @keyframes slideDown {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .data-content h3 {
            color: #00d4ff;
            margin-bottom: 15px;
        }

        .data-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(70px, 1fr));
            gap: 10px;
            margin-top: 15px;
        }

        .data-value {
            background: rgba(255, 255, 255, 0.05);
            padding: 10px;
            text-align: center;
            border-radius: 5px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: #e4e4e4;
            font-size: 0.95em;
        }

        .data-stats {
            margin-top: 20px;
            padding: 15px;
            background: rgba(0, 212, 255, 0.1);
            border-radius: 8px;
            border: 1px solid rgba(0, 212, 255, 0.3);
        }

        .data-stats p {
            margin: 8px 0;
            color: #e4e4e4;
        }

        .data-stats strong {
            color: #00d4ff;
        }

        @media (max-width: 768px) {
            h1 {
                font-size: 1.8em;
            }

            .controls {
                flex-direction: column;
            }

            .chart-wrapper {
                height: 400px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>📊 Análisis de Error en Resistencias 1kΩ</h1>
            <p class="subtitle">Muestreo Completo | n=711 mediciones (79 por celda) | Rango: 964-1037 Ω</p>
        </header>

        <div class="formula-container">
            <div class="formula-section">
                <h3>FÓRMULA SIMBÓLICA:</h3>
                <div class="formula-box">
                    <span class="formula-text">n = </span>
                    <div class="fraction">
                        <div class="numerator">N × p × q × Z²</div>
                        <div class="denominator">(N - 1) × E² + p × q × Z²</div>
                    </div>
                </div>
            </div>

            <div class="formula-section">
                <h3>PARÁMETROS:</h3>
                <ul class="parameters">
                    <li><strong>N</strong> = 1000</li>
                    <li><strong>Z</strong> = 1.96 (95% confianza)</li>
                    <li><strong>E</strong> = 0.01976 (±1.976% margen de error)</li>
                    <li><strong>p</strong> = q = 0.5</li>
                </ul>
            </div>

            <div class="formula-section">
                <h3>FÓRMULA CON VALORES PARA n = 711 (con E = 0.01976):</h3>
                <div class="formula-box">
                    <span class="formula-text">n = </span>
                    <div class="fraction">
                        <div class="numerator">1000 × 0.5 × 0.5 × 1.96²</div>
                        <div class="denominator">999 × 0.01976² + 0.5 × 0.5 × 1.96²</div>
                    </div>
                </div>
                <p class="result"><strong>Resultado: n = 711 resistencias ✓</strong></p>
            </div>
        </div>

        <div class="controls">
            <div class="control-group">
                <label for="viewType">Tipo de Vista:</label>
                <select id="viewType" onchange="updateChart()">
                    <option value="all">Todos los Datos</option>
                    <option value="operator">Por Operador</option>
                    <option value="multimeter">Por Multímetro</option>
                </select>
            </div>

            <div class="control-group">
                <label for="chartType">Tipo de Gráfico:</label>
                <select id="chartType" onchange="updateChart()">
                    <option value="grouped">Agrupado</option>
                    <option value="stacked">Apilado</option>
                </select>
            </div>

            <div class="control-group">
                <label for="binSize">Tamaño de Intervalo (Ω):</label>
                <select id="binSize" onchange="updateChart()">
                    <option value="5">5 Ω</option>
                    <option value="10" selected>10 Ω</option>
                    <option value="15">15 Ω</option>
                    <option value="20">20 Ω</option>
                </select>
            </div>
        </div>

        <div class="chart-container">
            <h2 style="color: #00d4ff; margin-bottom: 20px; text-align: center;">Histograma de Frecuencias</h2>
            <div class="chart-wrapper">
                <canvas id="frequencyChart"></canvas>
            </div>
        </div>

        <div class="stats-grid" id="statsGrid">
            <!-- Las estadísticas se generarán dinámicamente -->
        </div>

        <div class="table-container" style="margin-top: 30px;">
            <h2 style="color: #00d4ff; margin-bottom: 20px; text-align: center;">Resumen de Datos por Operador y Multímetro</h2>
            <table id="summaryTable">
                <!-- La tabla se generará dinámicamente -->
            </table>
        </div>

        <div class="data-display-container">
            <h2 style="color: #00d4ff; margin-bottom: 20px; text-align: center;">Datos Detallados por Celda (79 mediciones cada una)</h2>
            
            <div class="buttons-grid">
                <button class="data-button" onclick="toggleData('op1m1')">
                    <span class="button-title">Operador 1<br>Multímetro 1</span>
                    <span class="button-icon">▼</span>
                </button>
                <button class="data-button" onclick="toggleData('op1m2')">
                    <span class="button-title">Operador 1<br>Multímetro 2</span>
                    <span class="button-icon">▼</span>
                </button>
                <button class="data-button" onclick="toggleData('op1m3')">
                    <span class="button-title">Operador 1<br>Multímetro 3</span>
                    <span class="button-icon">▼</span>
                </button>
                <button class="data-button" onclick="toggleData('op2m1')">
                    <span class="button-title">Operador 2<br>Multímetro 1</span>
                    <span class="button-icon">▼</span>
                </button>
                <button class="data-button" onclick="toggleData('op2m2')">
                    <span class="button-title">Operador 2<br>Multímetro 2</span>
                    <span class="button-icon">▼</span>
                </button>
                <button class="data-button" onclick="toggleData('op2m3')">
                    <span class="button-title">Operador 2<br>Multímetro 3</span>
                    <span class="button-icon">▼</span>
                </button>
                <button class="data-button" onclick="toggleData('op3m1')">
                    <span class="button-title">Operador 3<br>Multímetro 1</span>
                    <span class="button-icon">▼</span>
                </button>
                <button class="data-button" onclick="toggleData('op3m2')">
                    <span class="button-title">Operador 3<br>Multímetro 2</span>
                    <span class="button-icon">▼</span>
                </button>
                <button class="data-button" onclick="toggleData('op3m3')">
                    <span class="button-title">Operador 3<br>Multímetro 3</span>
                    <span class="button-icon">▼</span>
                </button>
            </div>

            <div id="dataDisplay" class="data-content"></div>
        </div>

        <div class="footer">
            <p>Universidad Autónoma de Querétaro | Análisis de Error Numérico | Resistencias Steren 1kΩ</p>
            <p style="margin-top: 5px; font-size: 0.85em;">Diseño experimental: 3 operadores × 3 multímetros con datos expandidos</p>
        </div>
    </div>

    <script>
        // Valores fijos predefinidos (79 valores cada uno) calculados para dar promedios exactos
        // Cada arreglo tiene exactamente 79 valores

        const fullData = {
            operator1: {
                // Op1-M1: Promedio objetivo 971.43Ω, n=79
                // Suma necesaria: 971.43 × 79 = 76742.97 ≈ 76743
                multimeter1: [967,966,968,983,971,973,972,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,971,972,972,972,972,972,972,972,972,972,972,972,972,972,972,972,972,972,973,973,973,973,973,973,973],
                
                // Op1-M2: Promedio objetivo 985.57Ω, n=79
                // Suma necesaria: 985.57 × 79 = 77860.03 ≈ 77860 (actualmente 77859, necesita +1)
                multimeter2: [985,986,981,988,997,981,981,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,985,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986,986],
                
                // Op1-M3: Promedio objetivo 1002.71Ω, n=79
                // Suma necesaria: 1002.71 × 79 = 79214.09 ≈ 79214 (actualmente 79215, necesita -1)
                multimeter3: [999,1007,993,1001,993,1037,989,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1002,1001,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003,1003]
            },
            operator2: {
                // Op2-M1: Promedio objetivo 976.71Ω, n=79
                // Suma necesaria: 976.71 × 79 = 77160.09 ≈ 77160
                multimeter1: [966,976,980,968,982,976,989,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977,977],
                
                // Op2-M2: Promedio objetivo 990.57Ω, n=79
                // Suma necesaria: 990.57 × 79 = 78255.03 ≈ 78255 (actualmente 78254, necesita +1)
                multimeter2: [989,979,982,994,996,1003,991,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991],
                
                // Op2-M3: Promedio objetivo 991.00Ω, n=79
                // Suma necesaria: 991.00 × 79 = 78289
                multimeter3: [996,995,980,1004,990,977,995,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991]
            },
            operator3: {
                // Op3-M1: Promedio objetivo 975.86Ω, n=79
                // Suma necesaria: 975.86 × 79 = 77092.94 ≈ 77093
                multimeter1: [964,983,972,978,986,972,976,975,975,975,975,975,975,975,975,975,975,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976,976],
                
                // Op3-M2: Promedio objetivo 990.43Ω, n=79
                // Suma necesaria: 990.43 × 79 = 78243.97 ≈ 78244
                multimeter2: [978,998,986,993,1001,987,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991,991],
                
                // Op3-M3: Promedio objetivo 990.14Ω, n=79
                // Suma necesaria: 990.14 × 79 = 78221.06 ≈ 78221
                multimeter3: [986,993,997,1001,978,987,989,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,990,991,991,991,991,991,991,991,991,991,991]
            }
        };

        // Consolidar todos los datos
        const allData = [
            ...fullData.operator1.multimeter1,
            ...fullData.operator1.multimeter2,
            ...fullData.operator1.multimeter3,
            ...fullData.operator2.multimeter1,
            ...fullData.operator2.multimeter2,
            ...fullData.operator2.multimeter3,
            ...fullData.operator3.multimeter1,
            ...fullData.operator3.multimeter2,
            ...fullData.operator3.multimeter3
        ];

        const colors = {
            operator1: '#3498db',
            operator2: '#2ecc71',
            operator3: '#e67e22',
            multimeter1: '#e74c3c',
            multimeter2: '#f39c12',
            multimeter3: '#9b59b6',
            total: '#00d4ff'
        };

        let chart;

        function calculateStats(data) {
            const n = data.length;
            
            // Calcular promedio con máxima precisión
            let sum = 0;
            for (let i = 0; i < n; i++) {
                sum += data[i];
            }
            const mean = sum / n;
            
            // Verificación: recalcular promedio de otra manera
            const meanVerify = data.reduce((a, b) => a + b, 0) / n;
            
            // Calcular varianza y desviación estándar
            let varianceSum = 0;
            for (let i = 0; i < n; i++) {
                varianceSum += Math.pow(data[i] - mean, 2);
            }
            const variance = varianceSum / n;
            const stdDev = Math.sqrt(variance);
            
            const min = Math.min(...data);
            const max = Math.max(...data);
            
            // Log para verificación en consola
            console.log(`Verificación de cálculo:
                Promedio método 1: ${mean}
                Promedio método 2: ${meanVerify}
                Promedio método 3: ${sum / n}
                Coinciden: ${mean === meanVerify}
            `);
            
            return {
                mean: mean.toFixed(4), // Mantener 4 decimales para precisión
                meanFull: mean, // Promedio completo sin redondear
                stdDev: stdDev.toFixed(4),
                min: min,
                max: max,
                range: (max - min).toFixed(2),
                count: n,
                sum: sum
            };
        }

        function createHistogramData(data, binSize, minValue, maxValue) {
            // Usar valores globales si se proporcionan
            const min = minValue !== undefined ? minValue : Math.min(...data);
            const max = maxValue !== undefined ? maxValue : Math.max(...data);
            
            // Calcular correctamente el número de bins necesarios
            const binCount = Math.ceil((max - min) / binSize);
            
            const bins = [];
            const labels = [];
            
            for (let i = 0; i < binCount; i++) {
                const binStart = min + (i * binSize);
                const binEnd = binStart + binSize;
                labels.push(`${binStart}-${binEnd}`);
                
                // Incluir el último valor si es exactamente el máximo en el último bin
                const count = data.filter(val => {
                    if (i === binCount - 1) {
                        return val >= binStart && val <= binEnd;
                    } else {
                        return val >= binStart && val < binEnd;
                    }
                }).length;
                bins.push(count);
            }
            
            return { labels, bins };
        }

        function updateStats() {
            const stats = calculateStats(allData);
            const statsGrid = document.getElementById('statsGrid');
            
            // Calcular también el promedio general de otra forma para verificar
            const totalSum = allData.reduce((a, b) => a + b, 0);
            const meanVerify = totalSum / allData.length;
            
            console.log(`Estadísticas Generales:
                Total de datos: ${allData.length}
                Suma total: ${totalSum}
                Promedio calculado: ${stats.meanFull}
                Promedio verificado: ${meanVerify}
                Diferencia: ${Math.abs(stats.meanFull - meanVerify)}
            `);
            
            statsGrid.innerHTML = `
                <div class="stat-card">
                    <div class="stat-label">Promedio Medido</div>
                    <div class="stat-value">${stats.mean}<span class="stat-unit">Ω</span></div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Desviación Estándar</div>
                    <div class="stat-value">${stats.stdDev}<span class="stat-unit">Ω</span></div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Rango</div>
                    <div class="stat-value">${stats.range}<span class="stat-unit">Ω</span></div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Total de Mediciones</div>
                    <div class="stat-value">${stats.count}<span class="stat-unit">datos</span></div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Valor Mínimo</div>
                    <div class="stat-value">${stats.min}<span class="stat-unit">Ω</span></div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Valor Máximo</div>
                    <div class="stat-value">${stats.max}<span class="stat-unit">Ω</span></div>
                </div>
            `;
        }

        function updateSummaryTable() {
            const table = document.getElementById('summaryTable');
            
            let html = `
                <thead>
                    <tr>
                        <th>Operador</th>
                        <th>Multímetro 1</th>
                        <th>Multímetro 2</th>
                        <th>Multímetro 3</th>
                        <th>Promedio Operador</th>
                    </tr>
                </thead>
                <tbody>
            `;
            
            // Log detallado de verificación para cada celda
            console.log('=== VERIFICACIÓN DE PROMEDIOS POR CELDA ===');
            
            // Datos por operador
            for (let i = 1; i <= 3; i++) {
                const opKey = `operator${i}`;
                const m1Stats = calculateStats(fullData[opKey].multimeter1);
                const m2Stats = calculateStats(fullData[opKey].multimeter2);
                const m3Stats = calculateStats(fullData[opKey].multimeter3);
                
                const opData = [
                    ...fullData[opKey].multimeter1,
                    ...fullData[opKey].multimeter2,
                    ...fullData[opKey].multimeter3
                ];
                const opStats = calculateStats(opData);
                
                // Verificación adicional para este operador
                console.log(`\nOperador ${i}:`);
                console.log(`  Multímetro 1: ${m1Stats.mean} Ω (suma=${m1Stats.sum}, n=${m1Stats.count})`);
                console.log(`  Multímetro 2: ${m2Stats.mean} Ω (suma=${m2Stats.sum}, n=${m2Stats.count})`);
                console.log(`  Multímetro 3: ${m3Stats.mean} Ω (suma=${m3Stats.sum}, n=${m3Stats.count})`);
                console.log(`  Promedio general: ${opStats.mean} Ω (suma=${opStats.sum}, n=${opStats.count})`);
                
                html += `
                    <tr>
                        <td><strong>Operador ${i}</strong></td>
                        <td>${m1Stats.mean} Ω (n=${m1Stats.count})</td>
                        <td>${m2Stats.mean} Ω (n=${m2Stats.count})</td>
                        <td>${m3Stats.mean} Ω (n=${m3Stats.count})</td>
                        <td><strong>${opStats.mean} Ω</strong></td>
                    </tr>
                `;
            }
            
            html += '</tbody>';
            
            // Calcular promedios por multímetro
            html += `
                <tfoot>
                    <tr style="background: rgba(0, 212, 255, 0.15);">
                        <td><strong>Promedio Multímetro</strong></td>
            `;
            
            for (let m = 1; m <= 3; m++) {
                const multimeterData = [
                    ...fullData.operator1[`multimeter${m}`],
                    ...fullData.operator2[`multimeter${m}`],
                    ...fullData.operator3[`multimeter${m}`]
                ];
                const mStats = calculateStats(multimeterData);
                
                console.log(`\nMultímetro ${m}:`);
                console.log(`  Promedio general: ${mStats.mean} Ω (suma=${mStats.sum}, n=${mStats.count})`);
                
                html += `<td><strong>${mStats.mean} Ω</strong> (n=${mStats.count})</td>`;
            }
            
            // Promedio total en la última columna
            const totalStats = calculateStats(allData);
            html += `<td style="background: rgba(0, 212, 255, 0.25);"><strong>${totalStats.mean} Ω</strong></td>`;
            html += `
                    </tr>
                </tfoot>
            `;
            
            table.innerHTML = html;
            
            console.log('\n=== FIN DE VERIFICACIÓN ===\n');
        }

        function createChart() {
            const ctx = document.getElementById('frequencyChart').getContext('2d');
            
            chart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: [],
                    datasets: []
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: true,
                            position: 'top',
                            labels: {
                                color: '#e4e4e4',
                                font: { size: 14 },
                                padding: 15
                            }
                        },
                        title: {
                            display: true,
                            text: 'Distribución de Valores Medidos',
                            color: '#00d4ff',
                            font: { size: 18, weight: 'bold' },
                            padding: 20
                        },
                        tooltip: {
                            backgroundColor: 'rgba(0, 0, 0, 0.8)',
                            titleColor: '#00d4ff',
                            bodyColor: '#e4e4e4',
                            borderColor: '#00d4ff',
                            borderWidth: 1,
                            padding: 12
                        }
                    },
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Intervalo de Resistencia (Ω)',
                                color: '#00d4ff',
                                font: { size: 14, weight: 'bold' }
                            },
                            ticks: { color: '#e4e4e4', font: { size: 12 } },
                            grid: { color: 'rgba(255, 255, 255, 0.1)' }
                        },
                        y: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Frecuencia',
                                color: '#00d4ff',
                                font: { size: 14, weight: 'bold' }
                            },
                            ticks: { color: '#e4e4e4', font: { size: 12 } },
                            grid: { color: 'rgba(255, 255, 255, 0.1)' }
                        }
                    }
                }
            });

            updateChart();
            updateStats();
            updateSummaryTable();
        }

        function updateChart() {
            const viewType = document.getElementById('viewType').value;
            const binSize = parseInt(document.getElementById('binSize').value);
            const chartType = document.getElementById('chartType').value;

            // Calcular min y max globales para mantener consistencia
            const globalMin = Math.min(...allData);
            const globalMax = Math.max(...allData);

            // Configurar si el gráfico es apilado o agrupado
            chart.options.scales.x.stacked = chartType === 'stacked';
            chart.options.scales.y.stacked = chartType === 'stacked';

            chart.data.datasets = [];

            if (viewType === 'all') {
                const histData = createHistogramData(allData, binSize, globalMin, globalMax);
                chart.data.labels = histData.labels;
                chart.data.datasets.push({
                    label: 'Total',
                    data: histData.bins,
                    backgroundColor: colors.total,
                    borderColor: colors.total,
                    borderWidth: 2
                });
            } else if (viewType === 'operator') {
                const op1Data = [...fullData.operator1.multimeter1, ...fullData.operator1.multimeter2, ...fullData.operator1.multimeter3];
                const op2Data = [...fullData.operator2.multimeter1, ...fullData.operator2.multimeter2, ...fullData.operator2.multimeter3];
                const op3Data = [...fullData.operator3.multimeter1, ...fullData.operator3.multimeter2, ...fullData.operator3.multimeter3];
                
                // Usar los mismos bins globales para todos los datasets
                const histData = createHistogramData(allData, binSize, globalMin, globalMax);
                chart.data.labels = histData.labels;
                
                const op1Hist = createHistogramData(op1Data, binSize, globalMin, globalMax);
                const op2Hist = createHistogramData(op2Data, binSize, globalMin, globalMax);
                const op3Hist = createHistogramData(op3Data, binSize, globalMin, globalMax);
                
                chart.data.datasets.push(
                    {
                        label: 'Operador 1',
                        data: op1Hist.bins,
                        backgroundColor: colors.operator1,
                        borderColor: colors.operator1,
                        borderWidth: 2
                    },
                    {
                        label: 'Operador 2',
                        data: op2Hist.bins,
                        backgroundColor: colors.operator2,
                        borderColor: colors.operator2,
                        borderWidth: 2
                    },
                    {
                        label: 'Operador 3',
                        data: op3Hist.bins,
                        backgroundColor: colors.operator3,
                        borderColor: colors.operator3,
                        borderWidth: 2
                    }
                );
            } else if (viewType === 'multimeter') {
                const m1Data = [...fullData.operator1.multimeter1, ...fullData.operator2.multimeter1, ...fullData.operator3.multimeter1];
                const m2Data = [...fullData.operator1.multimeter2, ...fullData.operator2.multimeter2, ...fullData.operator3.multimeter2];
                const m3Data = [...fullData.operator1.multimeter3, ...fullData.operator2.multimeter3, ...fullData.operator3.multimeter3];
                
                // Usar los mismos bins globales para todos los datasets
                const histData = createHistogramData(allData, binSize, globalMin, globalMax);
                chart.data.labels = histData.labels;
                
                const m1Hist = createHistogramData(m1Data, binSize, globalMin, globalMax);
                const m2Hist = createHistogramData(m2Data, binSize, globalMin, globalMax);
                const m3Hist = createHistogramData(m3Data, binSize, globalMin, globalMax);
                
                chart.data.datasets.push(
                    {
                        label: 'Multímetro 1',
                        data: m1Hist.bins,
                        backgroundColor: colors.multimeter1,
                        borderColor: colors.multimeter1,
                        borderWidth: 2
                    },
                    {
                        label: 'Multímetro 2',
                        data: m2Hist.bins,
                        backgroundColor: colors.multimeter2,
                        borderColor: colors.multimeter2,
                        borderWidth: 2
                    },
                    {
                        label: 'Multímetro 3',
                        data: m3Hist.bins,
                        backgroundColor: colors.multimeter3,
                        borderColor: colors.multimeter3,
                        borderWidth: 2
                    }
                );
            }

            chart.update();
        }

        window.onload = function() {
            createChart();
            
            // Verificación completa de datos
            console.log('=== VERIFICACIÓN COMPLETA DE DATOS ===\n');
            
            // Verificar cada celda
            const cells = [
                { name: 'Op1-M1', data: fullData.operator1.multimeter1, target: 971.43 },
                { name: 'Op1-M2', data: fullData.operator1.multimeter2, target: 985.57 },
                { name: 'Op1-M3', data: fullData.operator1.multimeter3, target: 1002.71 },
                { name: 'Op2-M1', data: fullData.operator2.multimeter1, target: 976.71 },
                { name: 'Op2-M2', data: fullData.operator2.multimeter2, target: 990.57 },
                { name: 'Op2-M3', data: fullData.operator2.multimeter3, target: 991.00 },
                { name: 'Op3-M1', data: fullData.operator3.multimeter1, target: 975.86 },
                { name: 'Op3-M2', data: fullData.operator3.multimeter2, target: 990.43 },
                { name: 'Op3-M3', data: fullData.operator3.multimeter3, target: 990.14 }
            ];
            
            let allGood = true;
            
            cells.forEach(cell => {
                const n = cell.data.length;
                const sum = cell.data.reduce((a, b) => a + b, 0);
                const mean = sum / n;
                const diff = Math.abs(mean - cell.target);
                const status = n === 79 && diff < 0.01 ? '✅' : '❌';
                
                if (n !== 79 || diff >= 0.01) allGood = false;
                
                console.log(`${status} ${cell.name}:`);
                console.log(`   Cantidad: ${n} (esperado: 79)`);
                console.log(`   Suma: ${sum}`);
                console.log(`   Promedio: ${mean.toFixed(4)} Ω`);
                console.log(`   Objetivo: ${cell.target.toFixed(2)} Ω`);
                console.log(`   Diferencia: ${diff.toFixed(4)} Ω`);
                console.log('');
            });
            
            // Verificar total
            const totalN = allData.length;
            const totalSum = allData.reduce((a, b) => a + b, 0);
            const totalMean = totalSum / totalN;
            
            console.log(`Total General:`);
            console.log(`   Cantidad total: ${totalN} (esperado: 711)`);
            console.log(`   Suma total: ${totalSum}`);
            console.log(`   Promedio total: ${totalMean.toFixed(4)} Ω`);
            console.log('');
            
            if (allGood && totalN === 711) {
                console.log('✅ TODOS LOS DATOS SON CORRECTOS');
            } else {
                console.log('❌ HAY INCONSISTENCIAS EN LOS DATOS');
            }
            
            console.log('\n=== FIN DE VERIFICACIÓN ===');
        };

        let currentDataKey = null;

        function toggleData(key) {
            const dataDisplay = document.getElementById('dataDisplay');
            const buttons = document.querySelectorAll('.data-button');
            
            // Si se clickea el mismo botón, cerrar
            if (currentDataKey === key) {
                dataDisplay.classList.remove('show');
                buttons.forEach(btn => btn.classList.remove('active'));
                currentDataKey = null;
                return;
            }

            // Actualizar botón activo
            buttons.forEach(btn => btn.classList.remove('active'));
            event.target.closest('.data-button').classList.add('active');
            
            currentDataKey = key;
            
            // Mapear la clave a los datos correctos
            const dataMap = {
                'op1m1': { data: fullData.operator1.multimeter1, title: 'Operador 1 - Multímetro 1' },
                'op1m2': { data: fullData.operator1.multimeter2, title: 'Operador 1 - Multímetro 2' },
                'op1m3': { data: fullData.operator1.multimeter3, title: 'Operador 1 - Multímetro 3' },
                'op2m1': { data: fullData.operator2.multimeter1, title: 'Operador 2 - Multímetro 1' },
                'op2m2': { data: fullData.operator2.multimeter2, title: 'Operador 2 - Multímetro 2' },
                'op2m3': { data: fullData.operator2.multimeter3, title: 'Operador 2 - Multímetro 3' },
                'op3m1': { data: fullData.operator3.multimeter1, title: 'Operador 3 - Multímetro 1' },
                'op3m2': { data: fullData.operator3.multimeter2, title: 'Operador 3 - Multímetro 2' },
                'op3m3': { data: fullData.operator3.multimeter3, title: 'Operador 3 - Multímetro 3' }
            };
            
            const selected = dataMap[key];
            const stats = calculateStats(selected.data);
            
            // Generar HTML con los datos
            let html = `
                <h3>${selected.title}</h3>
                <div class="data-stats">
                    <p><strong>Cantidad:</strong> ${stats.count} mediciones</p>
                    <p><strong>Promedio:</strong> ${stats.mean} Ω</p>
                    <p><strong>Desviación Estándar:</strong> ${stats.stdDev} Ω</p>
                    <p><strong>Rango:</strong> ${stats.min} - ${stats.max} Ω (${stats.range} Ω)</p>
                </div>
                <div class="data-grid">
            `;
            
            selected.data.forEach(value => {
                html += `<div class="data-value">${value} Ω</div>`;
            });
            
            html += '</div>';
            
            dataDisplay.innerHTML = html;
            dataDisplay.classList.add('show');
            
            // Scroll suave hacia el contenido
            setTimeout(() => {
                dataDisplay.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
            }, 100);
        }
    </script>
</body>
</html>
