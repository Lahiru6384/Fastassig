<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Management System</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css">
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-database-compat.js"></script>
    <style>
        :root {
            --bg: #0f172a;
            --bg-secondary: #111827;
            --panel: rgba(15, 23, 42, 0.84);
            --panel-soft: rgba(30, 41, 59, 0.92);
            --border: rgba(148, 163, 184, 0.16);
            --text: #e2e8f0;
            --text-soft: #94a3b8;
            --white: #f8fafc;
            --blue: #3b82f6;
            --blue-deep: #2563eb;
            --emerald: #10b981;
            --orange: #f97316;
            --red: #ef4444;
            --red-bright: #ff4d4f;
            --grey-dark: #475569;
            --shadow: 0 20px 50px rgba(2, 6, 23, 0.4);
            --radius-xl: 28px;
            --radius-lg: 20px;
            --radius-md: 14px;
        }

        * {
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            margin: 0;
            min-height: 100vh;
            font-family: 'Poppins', sans-serif;
            color: var(--text);
            background:
                radial-gradient(circle at top left, rgba(59, 130, 246, 0.22), transparent 28%),
                radial-gradient(circle at top right, rgba(16, 185, 129, 0.14), transparent 24%),
                linear-gradient(160deg, #020617 0%, #0f172a 48%, #1e293b 100%);
            padding: 24px;
        }

        body::before {
            content: "";
            position: fixed;
            inset: 0;
            pointer-events: none;
            background:
                linear-gradient(rgba(255, 255, 255, 0.02), rgba(255, 255, 255, 0.01)),
                repeating-linear-gradient(
                    135deg,
                    rgba(148, 163, 184, 0.04) 0,
                    rgba(148, 163, 184, 0.04) 1px,
                    transparent 1px,
                    transparent 18px
                );
        }

        .container {
            max-width: 1220px;
            margin: 0 auto;
            animation: fadeInUp 0.7s ease both;
        }

        .auth-screen {
            min-height: calc(100vh - 48px);
            display: grid;
            place-items: center;
        }

        .auth-card {
            width: min(100%, 460px);
            padding: 28px;
            border-radius: 28px;
            border: 1px solid var(--border);
            background: linear-gradient(180deg, rgba(30, 41, 59, 0.88), rgba(15, 23, 42, 0.94));
            box-shadow: var(--shadow);
            backdrop-filter: blur(18px);
        }

        .auth-brand {
            text-align: center;
            margin-bottom: 24px;
        }

        .auth-title {
            margin: 0 0 8px;
            font-size: 1.8rem;
            font-weight: 800;
            color: var(--white);
        }

        .auth-text {
            margin: 0;
            color: var(--text-soft);
            line-height: 1.6;
        }

        .auth-form {
            display: grid;
            gap: 16px;
        }

        .auth-error {
            min-height: 22px;
            color: #fca5a5;
            font-size: 0.88rem;
            font-weight: 600;
        }

        .hidden {
            display: none !important;
        }

        .dashboard {
            background: linear-gradient(180deg, rgba(15, 23, 42, 0.92), rgba(15, 23, 42, 0.82));
            border: 1px solid var(--border);
            border-radius: var(--radius-xl);
            box-shadow: var(--shadow);
            overflow: hidden;
            backdrop-filter: blur(18px);
        }

        .hero {
            display: flex;
            justify-content: space-between;
            gap: 20px;
            align-items: flex-start;
            padding: 30px 30px 18px;
            border-bottom: 1px solid var(--border);
            background: linear-gradient(135deg, rgba(30, 41, 59, 0.9), rgba(15, 23, 42, 0.78));
        }

        .hero-brand {
            display: flex;
            align-items: center;
            gap: 18px;
            flex-wrap: wrap;
        }

        .hero h1 {
            margin: 0 0 10px;
            font-size: clamp(1.9rem, 3vw, 2.8rem);
            font-weight: 800;
            letter-spacing: -0.04em;
            color: var(--white);
        }

        .hero p {
            margin: 0;
            max-width: 760px;
            line-height: 1.7;
            color: var(--text-soft);
        }

        .hero-chip {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            padding: 12px 16px;
            border-radius: 999px;
            background: rgba(59, 130, 246, 0.14);
            border: 1px solid rgba(96, 165, 250, 0.22);
            color: #dbeafe;
            font-size: 0.92rem;
            font-weight: 600;
            white-space: nowrap;
        }

        .hero-tools {
            display: flex;
            align-items: center;
            justify-content: flex-end;
            gap: 12px;
            flex-wrap: wrap;
        }

        .tool-btn {
            min-height: auto;
            padding: 10px 14px;
            border-radius: 999px;
            background: rgba(15, 23, 42, 0.68);
            border: 1px solid rgba(148, 163, 184, 0.18);
            box-shadow: none;
        }

        .tool-btn .count {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            min-width: 22px;
            height: 22px;
            padding: 0 6px;
            border-radius: 999px;
            background: #ef4444;
            color: #fff;
            font-size: 0.76rem;
            font-weight: 800;
        }

        .notice-panel {
            margin-bottom: 22px;
            padding: 18px 20px;
            border-radius: 18px;
            border: 1px solid rgba(96, 165, 250, 0.18);
            background: linear-gradient(180deg, rgba(30, 41, 59, 0.82), rgba(15, 23, 42, 0.88));
        }

        .notice-head {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 12px;
            margin-bottom: 12px;
        }

        .notice-title {
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--white);
            font-weight: 700;
        }

        .notice-list {
            display: grid;
            gap: 10px;
        }

        .notice-item {
            padding: 12px 14px;
            border-radius: 14px;
            background: rgba(15, 23, 42, 0.6);
            border: 1px solid rgba(148, 163, 184, 0.12);
            color: var(--text);
            font-size: 0.92rem;
            line-height: 1.5;
        }

        .notice-item strong {
            color: var(--white);
        }

        .report-panel {
            margin-top: 24px;
            padding: 24px;
            border-radius: var(--radius-lg);
            border: 1px solid var(--border);
            background: linear-gradient(180deg, rgba(30, 41, 59, 0.78), rgba(15, 23, 42, 0.92));
        }

        .report-head {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            gap: 16px;
            margin-bottom: 18px;
        }

        .report-head h2 {
            margin: 0 0 6px;
            font-size: 1.1rem;
            color: var(--white);
        }

        .report-head p {
            margin: 0;
            color: var(--text-soft);
            font-size: 0.9rem;
        }

        .report-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 14px;
            margin-bottom: 18px;
        }

        .report-card {
            padding: 16px;
            border-radius: 16px;
            border: 1px solid var(--border);
            background: rgba(15, 23, 42, 0.66);
        }

        .report-card-label {
            color: var(--text-soft);
            font-size: 0.84rem;
            margin-bottom: 8px;
        }

        .report-card-value {
            color: var(--white);
            font-size: 1.4rem;
            font-weight: 800;
        }

        .report-table-wrap {
            overflow-x: auto;
        }

        .report-table {
            width: 100%;
            min-width: 900px;
            border-collapse: collapse;
        }

        .report-table th,
        .report-table td {
            padding: 12px 10px;
            border-top: 1px solid rgba(148, 163, 184, 0.1);
            text-align: left;
            color: var(--text);
            font-size: 0.9rem;
        }

        .report-table th {
            color: #cbd5e1;
            font-size: 0.78rem;
            text-transform: uppercase;
            letter-spacing: 0.08em;
            background: rgba(15, 23, 42, 0.92);
        }

        .sync-panel {
            margin-top: 24px;
            padding: 22px;
            border-radius: var(--radius-lg);
            border: 1px solid var(--border);
            background: linear-gradient(180deg, rgba(30, 41, 59, 0.78), rgba(15, 23, 42, 0.92));
        }

        .sync-panel h3 {
            margin: 0 0 8px;
            color: var(--white);
            font-size: 1.08rem;
        }

        .sync-panel p {
            margin: 0 0 14px;
            color: var(--text-soft);
            font-size: 0.9rem;
        }

        .sync-row {
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
            margin-bottom: 16px;
        }

        .sync-row input {
            flex: 1 1 260px;
        }

        .sync-list {
            list-style: none;
            margin: 0;
            padding: 0;
            display: grid;
            gap: 10px;
        }

        .sync-list li {
            padding: 12px 14px;
            border-radius: 14px;
            background: rgba(15, 23, 42, 0.66);
            border: 1px solid rgba(148, 163, 184, 0.12);
            color: var(--text);
        }

        .section {
            padding: 24px 30px 30px;
        }

        .cards,
        .form-grid {
            display: grid;
            gap: 16px;
        }

        .cards {
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            margin-bottom: 22px;
        }

        .card,
        .field,
        .table-panel {
            background: linear-gradient(180deg, rgba(30, 41, 59, 0.78), rgba(15, 23, 42, 0.92));
            border: 1px solid var(--border);
            border-radius: var(--radius-lg);
            box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.03);
        }

        .card {
            padding: 20px;
        }

        .card-label {
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--text-soft);
            font-size: 0.95rem;
            margin-bottom: 10px;
        }

        .card-value {
            font-size: 2rem;
            font-weight: 800;
            color: var(--white);
            letter-spacing: -0.04em;
        }

        .card-note {
            margin-top: 8px;
            font-size: 0.84rem;
            color: var(--text-soft);
        }

        .metric-bar {
            margin-top: 14px;
        }

        .metric-track {
            width: 100%;
            height: 10px;
            border-radius: 999px;
            background: rgba(148, 163, 184, 0.16);
            overflow: hidden;
        }

        .metric-fill {
            height: 100%;
            width: 0;
            border-radius: inherit;
            transition: width 0.35s ease;
        }

        .metric-fill-contract {
            background: linear-gradient(135deg, #60a5fa, #2563eb);
        }

        .metric-fill-received {
            background: linear-gradient(135deg, #34d399, #059669);
        }

        .metric-fill-receivable {
            background: linear-gradient(135deg, #fb923c, #f97316);
        }

        .metric-meta {
            margin-top: 8px;
            display: flex;
            justify-content: space-between;
            gap: 12px;
            font-size: 0.78rem;
            color: var(--text-soft);
        }

        .form-grid {
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            margin-bottom: 24px;
        }

        .field {
            padding: 18px;
        }

        label {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 10px;
            color: var(--white);
            font-size: 0.93rem;
            font-weight: 600;
        }

        .label-icon {
            width: 18px;
            color: #93c5fd;
            text-align: center;
        }

        input,
        select {
            width: 100%;
            padding: 14px 15px;
            border-radius: var(--radius-md);
            border: 1px solid rgba(100, 116, 139, 0.28);
            background: rgba(15, 23, 42, 0.9);
            color: var(--white);
            font: inherit;
            outline: none;
            transition: border-color 0.2s ease, box-shadow 0.2s ease, transform 0.2s ease;
        }

        input::placeholder {
            color: #64748b;
        }

        input:focus,
        select:focus {
            border-color: rgba(59, 130, 246, 0.6);
            box-shadow: 0 0 0 4px rgba(59, 130, 246, 0.16);
            transform: translateY(-1px);
        }

        input[readonly] {
            background: rgba(30, 41, 59, 0.88);
            color: #cbd5e1;
            font-weight: 700;
        }

        .actions {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            grid-column: 1 / -1;
        }

        button {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            border: none;
            border-radius: 16px;
            min-height: 52px;
            padding: 14px 18px;
            font: inherit;
            font-weight: 700;
            color: var(--white);
            cursor: pointer;
            transition: transform 0.2s ease, filter 0.2s ease, box-shadow 0.2s ease;
            box-shadow: 0 12px 24px rgba(2, 6, 23, 0.32);
        }

        button:hover {
            transform: translateY(-2px);
            filter: brightness(1.06);
        }

        button:active {
            transform: translateY(0);
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--blue), var(--blue-deep));
        }

        .btn-secondary {
            background: linear-gradient(135deg, var(--emerald), #059669);
        }

        .btn-info {
            min-height: auto;
            padding: 8px 12px;
            border-radius: 10px;
            background: linear-gradient(135deg, #3b82f6, #2563eb);
            box-shadow: none;
        }

        .btn-success {
            min-height: auto;
            padding: 8px 12px;
            border-radius: 10px;
            background: linear-gradient(135deg, #10b981, #059669);
            box-shadow: none;
        }

        .btn-danger {
            min-height: auto;
            padding: 8px 12px;
            border-radius: 10px;
            background: linear-gradient(135deg, #f97316, #ef4444);
            box-shadow: none;
        }

        .tab-bar {
            display: inline-flex;
            gap: 10px;
            padding: 6px;
            border-radius: 16px;
            background: rgba(15, 23, 42, 0.7);
            border: 1px solid var(--border);
        }

        .tab-btn {
            min-height: auto;
            padding: 10px 16px;
            border-radius: 12px;
            background: transparent;
            color: var(--text-soft);
            box-shadow: none;
        }

        .tab-btn.active {
            background: linear-gradient(135deg, rgba(59, 130, 246, 0.24), rgba(37, 99, 235, 0.32));
            color: var(--white);
        }

        .table-panel {
            overflow: hidden;
        }

        .table-head {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 14px;
            padding: 20px 20px 14px;
            border-bottom: 1px solid var(--border);
        }

        .table-head h2 {
            margin: 0;
            font-size: 1.1rem;
            color: var(--white);
        }

        .table-head p {
            margin: 5px 0 0;
            font-size: 0.9rem;
            color: var(--text-soft);
        }

        .table-wrap {
            overflow-x: auto;
        }

        table {
            width: 100%;
            min-width: 980px;
            border-collapse: collapse;
        }

        thead th {
            text-align: left;
            padding: 15px 14px;
            background: rgba(15, 23, 42, 0.95);
            color: #cbd5e1;
            font-size: 0.78rem;
            text-transform: uppercase;
            letter-spacing: 0.08em;
            white-space: nowrap;
        }

        tbody td {
            padding: 15px 14px;
            border-top: 1px solid rgba(148, 163, 184, 0.1);
            color: var(--text);
            vertical-align: middle;
            background: rgba(15, 23, 42, 0.82);
        }

        tbody tr {
            transition: background 0.2s ease, transform 0.2s ease;
        }

        tbody tr:hover {
            background: rgba(51, 65, 85, 0.45);
        }

        tbody tr:hover td {
            background: rgba(30, 41, 59, 0.9);
        }

        .muted {
            color: var(--text-soft);
        }

        .badge {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            padding: 7px 12px;
            border-radius: 999px;
            font-size: 0.78rem;
            font-weight: 700;
            white-space: nowrap;
        }

        .badge-assignment {
            background: rgba(59, 130, 246, 0.24);
            color: #dbeafe;
        }

        .badge-project {
            background: rgba(168, 85, 247, 0.18);
            color: #e9d5ff;
        }

        .badge-paid {
            background: rgba(16, 185, 129, 0.16);
            color: #a7f3d0;
        }

        .badge-due {
            background: rgba(249, 115, 22, 0.22);
            color: #fed7aa;
        }

        .money {
            font-variant-numeric: tabular-nums;
            font-weight: 600;
            white-space: nowrap;
        }

        .date-normal {
            color: #cbd5e1;
            font-weight: 600;
        }

        .date-urgent {
            color: var(--red-bright);
            font-weight: 700;
        }

        .date-overdue {
            color: #94a3b8;
            font-weight: 700;
        }

        .row-urgent {
            background: linear-gradient(90deg, rgba(239, 68, 68, 0.14), rgba(239, 68, 68, 0.04));
        }

        .row-overdue {
            background: linear-gradient(90deg, rgba(71, 85, 105, 0.22), rgba(71, 85, 105, 0.08));
        }

        .row-urgent td {
            background: rgba(69, 18, 26, 0.72);
        }

        .row-overdue td {
            background: rgba(39, 49, 63, 0.86);
        }

        .empty-state {
            padding: 36px 20px 42px;
            text-align: center;
            color: var(--text-soft);
        }

        .empty-state i {
            font-size: 2rem;
            color: #60a5fa;
            margin-bottom: 12px;
        }

        .actions-cell {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }

        .actions-cell button {
            min-height: auto;
            padding: 7px 11px;
            border-radius: 10px;
            font-size: 0.82rem;
            font-weight: 700;
            gap: 7px;
        }

        .finance-panel {
            padding: 22px 20px 24px;
        }

        .finance-grid {
            display: grid;
            gap: 18px;
        }

        .finance-item {
            padding: 18px;
            border-radius: 18px;
            border: 1px solid var(--border);
            background: rgba(15, 23, 42, 0.72);
        }

        .finance-top {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 16px;
            margin-bottom: 12px;
        }

        .finance-label {
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--white);
            font-weight: 600;
        }

        .finance-value {
            font-size: 1.25rem;
            font-weight: 700;
            color: var(--white);
            white-space: nowrap;
        }

        .finance-caption {
            margin-top: 8px;
            font-size: 0.82rem;
            color: var(--text-soft);
        }

        .bottom-finance {
            margin-top: 24px;
            padding: 26px;
            border-radius: var(--radius-lg);
            border: 1px solid var(--border);
            background: linear-gradient(180deg, rgba(30, 41, 59, 0.78), rgba(15, 23, 42, 0.92));
        }

        .bottom-finance-head {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            gap: 18px;
            margin-bottom: 20px;
        }

        .bottom-finance-head h2 {
            margin: 0 0 6px;
            font-size: 1.1rem;
            color: var(--white);
        }

        .bottom-finance-head p {
            margin: 0;
            color: var(--text-soft);
            font-size: 0.9rem;
        }

        .finance-chip {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            padding: 10px 14px;
            border-radius: 999px;
            background: rgba(59, 130, 246, 0.14);
            border: 1px solid rgba(96, 165, 250, 0.2);
            color: #dbeafe;
            font-size: 0.84rem;
            font-weight: 600;
            white-space: nowrap;
        }

        .finance-strip {
            display: grid;
            grid-template-columns: 1.3fr 1fr;
            gap: 22px;
            align-items: stretch;
        }

        .finance-main {
            padding: 22px;
            border-radius: 20px;
            border: 1px solid var(--border);
            background: rgba(15, 23, 42, 0.65);
        }

        .finance-main-label {
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--white);
            font-weight: 600;
            margin-bottom: 10px;
        }

        .finance-main-value {
            font-size: clamp(2rem, 4vw, 3rem);
            font-weight: 800;
            line-height: 1;
            color: var(--white);
            margin-bottom: 12px;
        }

        .finance-main-note {
            color: var(--text-soft);
            font-size: 0.92rem;
            line-height: 1.6;
            margin-bottom: 16px;
        }

        .finance-side {
            display: grid;
            gap: 14px;
        }

        .finance-mini {
            padding: 18px;
            border-radius: 18px;
            border: 1px solid var(--border);
            background: rgba(15, 23, 42, 0.65);
        }

        .finance-mini-top {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 14px;
            margin-bottom: 10px;
        }

        .finance-mini-label {
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--white);
            font-weight: 600;
        }

        .finance-mini-value {
            font-size: 1.35rem;
            font-weight: 800;
            color: var(--white);
            white-space: nowrap;
        }

        .finance-mini-note {
            margin-top: 8px;
            color: var(--text-soft);
            font-size: 0.82rem;
            line-height: 1.5;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(18px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @media (max-width: 900px) {
            .hero {
                flex-direction: column;
            }
        }

        @media (max-width: 768px) {
            body {
                padding: 16px;
            }

            .hero,
            .section {
                padding-left: 18px;
                padding-right: 18px;
            }

            .cards {
                grid-template-columns: 1fr;
            }

            table {
                min-width: 900px;
            }

            .bottom-finance-head,
            .finance-strip {
                grid-template-columns: 1fr;
                display: grid;
            }
        }

        @media (max-width: 520px) {
            .actions {
                flex-direction: column;
            }

            .actions button {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <section class="auth-screen" id="authScreen">
            <div class="auth-card">
                <div class="auth-brand">
                    <h1 class="auth-title">Fast Assignment</h1>
                    <p class="auth-text">Sign in to open the task management system and continue your daily workflow.</p>
                </div>

                <form class="auth-form" id="loginForm">
                    <div class="field">
                        <label for="loginUsername"><i class="fa-solid fa-user-lock label-icon"></i> Username</label>
                        <input type="text" id="loginUsername" placeholder="Enter username" autocomplete="username">
                    </div>

                    <div class="field">
                        <label for="loginPassword"><i class="fa-solid fa-key label-icon"></i> Password</label>
                        <input type="password" id="loginPassword" placeholder="Enter password" autocomplete="current-password">
                    </div>

                    <div class="auth-error" id="loginError"></div>

                    <button type="submit" class="btn-primary">
                        <i class="fa-solid fa-right-to-bracket"></i>
                        <span>Login</span>
                    </button>
                </form>
            </div>
        </section>

        <div class="dashboard hidden" id="appShell">
            <header class="hero">
                <div>
                    <div class="hero-brand">
                        <div>
                            <h1><i class="fa-solid fa-clipboard-check"></i> Fast Assignment Task Management System</h1>
                            <p>Manage student assignments, assessments, and projects with stable task IDs, smart due-date highlighting, persistent browser storage, and polished export-ready workflow tools for Fast Assignment.</p>
                        </div>
                    </div>
                </div>
                <div class="hero-tools">
                    <div class="hero-chip">
                        <i class="fa-solid fa-shield-halved"></i>
                        <span id="roleBadge">Fast Assignment Dashboard</span>
                    </div>
                    <button type="button" class="tool-btn hidden" id="notificationBtn">
                        <i class="fa-solid fa-bell"></i>
                        <span>Notifications</span>
                        <span class="count" id="notificationCount">0</span>
                    </button>
                    <button type="button" class="tool-btn" id="logoutBtn">
                        <i class="fa-solid fa-right-from-bracket"></i>
                        <span>Logout</span>
                    </button>
                </div>
            </header>

            <main class="section">
                <section class="notice-panel hidden" id="notificationPanel">
                    <div class="notice-head">
                        <div class="notice-title">
                            <i class="fa-solid fa-bell"></i>
                            <span>Admin Notifications</span>
                        </div>
                        <button type="button" class="tool-btn" id="clearNotificationsBtn">
                            <i class="fa-solid fa-check-double"></i>
                            <span>Clear</span>
                        </button>
                    </div>
                    <div class="notice-list" id="notificationList"></div>
                </section>

                <section class="cards" aria-label="Summary cards">
                    <div class="card">
                        <div class="card-label"><i class="fa-solid fa-list-check"></i> Total Tasks</div>
                        <div class="card-value" id="totalTasks">0</div>
                        <div class="card-note">All active, pending, and completed records</div>
                    </div>
                    <div class="card">
                        <div class="card-label"><i class="fa-solid fa-circle-check"></i> Completed</div>
                        <div class="card-value" id="completedTasks">0</div>
                        <div class="card-note">Fully settled finished work</div>
                    </div>
                    <div class="card">
                        <div class="card-label"><i class="fa-solid fa-hourglass-half"></i> Pending Payments</div>
                        <div class="card-value" id="pendingPayments">0</div>
                        <div class="card-note">Finished work waiting for collection</div>
                    </div>
                </section>

                <section class="form-grid" aria-label="Task form">
                    <div class="field">
                        <label for="name"><i class="fa-solid fa-user label-icon"></i> Client Name</label>
                        <input type="text" id="name" placeholder="Enter student or client name">
                    </div>

                    <div class="field">
                        <label for="date"><i class="fa-solid fa-calendar-days label-icon"></i> Due Date</label>
                        <input type="date" id="date">
                    </div>

                    <div class="field">
                        <label for="type"><i class="fa-solid fa-layer-group label-icon"></i> Task Type</label>
                        <select id="type">
                            <option>Assignment</option>
                            <option>Assessment</option>
                            <option>Case Study</option>
                            <option>Research</option>
                            <option>Presentation</option>
                            <option>Report</option>
                            <option>Dissertation</option>
                            <option>Thesis</option>
                            <option>Essay</option>
                            <option>Project</option>
                        </select>
                    </div>

                    <div class="field">
                        <label for="full"><i class="fa-solid fa-money-bill-trend-up label-icon"></i> Full Payment</label>
                        <input type="number" id="full" min="0" step="0.01" placeholder="0.00">
                    </div>

                    <div class="field">
                        <label for="adv"><i class="fa-solid fa-hand-holding-dollar label-icon"></i> Advance</label>
                        <input type="number" id="adv" min="0" step="0.01" placeholder="0.00">
                    </div>

                    <div class="field">
                        <label for="bal"><i class="fa-solid fa-scale-balanced label-icon"></i> Balance</label>
                        <input type="number" id="bal" readonly placeholder="0.00">
                    </div>

                    <div class="actions">
                        <button type="button" id="saveBtn" class="btn-primary">
                            <i class="fa-solid fa-floppy-disk"></i>
                            <span>Save Task</span>
                        </button>
                        <button type="button" id="reportBtn" class="btn-secondary">
                            <i class="fa-solid fa-chart-column"></i>
                            <span>Generate Full Report</span>
                        </button>
                    </div>
                </section>

                <section class="table-panel">
                    <div class="table-head">
                        <div>
                            <h2 id="tableTitle">Active Tasks</h2>
                            <p id="tableSubtitle">Responsive task table with due-date status, task IDs, and payment tracking.</p>
                        </div>
                        <div class="tab-bar" aria-label="Task tabs">
                            <button type="button" class="tab-btn active" data-tab="active" id="activeTabBtn">
                                <i class="fa-solid fa-list"></i>
                                <span>Active Tasks</span>
                            </button>
                            <button type="button" class="tab-btn" data-tab="pending" id="pendingTabBtn">
                                <i class="fa-solid fa-wallet"></i>
                                <span>Pending Payments</span>
                            </button>
                            <button type="button" class="tab-btn" data-tab="completed" id="completedTabBtn">
                                <i class="fa-solid fa-circle-check"></i>
                                <span>Completed List</span>
                            </button>
                        </div>
                    </div>

                    <div class="table-wrap">
                        <table id="taskTable">
                            <thead>
                                <tr>
                                    <th>Task ID</th>
                                    <th>Name</th>
                                    <th>Due Date</th>
                                    <th>Type</th>
                                    <th>Full</th>
                                    <th>Advance</th>
                                    <th>Balance</th>
                                    <th>Status</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody id="tableBody"></tbody>
                        </table>
                        <div class="empty-state" id="emptyState">
                            <i class="fa-regular fa-folder-open"></i>
                            <div id="emptyStateText">No tasks yet. Save your first task to populate the report dashboard.</div>
                        </div>
                    </div>
                </section>

                <section class="bottom-finance" id="bottomFinanceSection">
                    <div class="bottom-finance-head">
                        <div>
                            <h2>Accounts Summary</h2>
                            <p>Bottom-page financial overview for contract value, collected payments, and remaining receivables.</p>
                        </div>
                        <div class="finance-chip">
                            <i class="fa-solid fa-chart-line"></i>
                            <span>Live Financial Snapshot</span>
                        </div>
                    </div>
                    <div class="finance-strip">
                        <div class="finance-main">
                            <div class="finance-main-label"><i class="fa-solid fa-wallet"></i> Total Receivable</div>
                            <div class="finance-main-value" id="financeReceivableValue">0.00</div>
                            <div class="finance-main-note">This is the total outstanding balance still left to collect from all customers across active and finished work.</div>
                            <div class="metric-track">
                                <div class="metric-fill metric-fill-receivable" id="financeReceivableBar"></div>
                            </div>
                            <div class="metric-meta">
                                <span>Still to be collected</span>
                                <span id="financeReceivableBarLabel">0%</span>
                            </div>
                        </div>
                        <div class="finance-side">
                            <div class="finance-mini">
                                <div class="finance-mini-top">
                                    <div class="finance-mini-label"><i class="fa-solid fa-hand-holding-dollar"></i> Total Received</div>
                                    <div class="finance-mini-value" id="financeReceivedValue">0.00</div>
                                </div>
                                <div class="metric-track">
                                    <div class="metric-fill metric-fill-received" id="financeReceivedBar"></div>
                                </div>
                                <div class="metric-meta">
                                    <span>Collected against total value</span>
                                    <span id="financeReceivedBarLabel">0%</span>
                                </div>
                                <div class="finance-mini-note">All advance and follow-up payments already collected from customers.</div>
                            </div>

                            <div class="finance-mini">
                                <div class="finance-mini-top">
                                    <div class="finance-mini-label"><i class="fa-solid fa-file-invoice-dollar"></i> Total Contract Value</div>
                                    <div class="finance-mini-value" id="financeContractValue">0.00</div>
                                </div>
                                <div class="metric-track">
                                    <div class="metric-fill metric-fill-contract" id="financeContractBar"></div>
                                </div>
                                <div class="metric-meta">
                                    <span>Overall business value</span>
                                    <span id="financeContractBarLabel">0%</span>
                                </div>
                                <div class="finance-mini-note">This is the full billing value of all assignments and projects currently stored in the system.</div>
                            </div>
                        </div>
                    </div>
                </section>

                <section class="report-panel hidden" id="reportSection">
                    <div class="report-head">
                        <div>
                            <h2>Full Business Report</h2>
                            <p id="reportGeneratedAt">Generated on demand from the current task records.</p>
                        </div>
                        <button type="button" class="tool-btn" id="closeReportBtn">
                            <i class="fa-solid fa-xmark"></i>
                            <span>Close Report</span>
                        </button>
                    </div>

                    <div class="report-grid">
                        <div class="report-card">
                            <div class="report-card-label">Total Tasks</div>
                            <div class="report-card-value" id="reportTotalTasks">0</div>
                        </div>
                        <div class="report-card">
                            <div class="report-card-label">Active Tasks</div>
                            <div class="report-card-value" id="reportActiveTasks">0</div>
                        </div>
                        <div class="report-card">
                            <div class="report-card-label">Pending Payments</div>
                            <div class="report-card-value" id="reportPendingTasks">0</div>
                        </div>
                        <div class="report-card">
                            <div class="report-card-label">Completed Tasks</div>
                            <div class="report-card-value" id="reportCompletedTasks">0</div>
                        </div>
                        <div class="report-card">
                            <div class="report-card-label">Total Contract Value</div>
                            <div class="report-card-value" id="reportContractValue">0.00</div>
                        </div>
                        <div class="report-card">
                            <div class="report-card-label">Total Received</div>
                            <div class="report-card-value" id="reportReceivedValue">0.00</div>
                        </div>
                        <div class="report-card">
                            <div class="report-card-label">Total Receivable</div>
                            <div class="report-card-value" id="reportReceivableValue">0.00</div>
                        </div>
                    </div>

                    <div class="report-table-wrap">
                        <table class="report-table">
                            <thead>
                                <tr>
                                    <th>Task ID</th>
                                    <th>Name</th>
                                    <th>Due Date</th>
                                    <th>Type</th>
                                    <th>Status</th>
                                    <th>Full</th>
                                    <th>Advance</th>
                                    <th>Balance</th>
                                </tr>
                            </thead>
                            <tbody id="reportTableBody"></tbody>
                        </table>
                    </div>
                </section>

                <section class="sync-panel">
                    <h3>Add New Client</h3>
                    <p>Cloud sync section powered by Firebase Realtime Database.</p>
                    <div class="sync-row">
                        <input type="text" id="clientInput" placeholder="Enter client name">
                        <button type="button" class="btn-secondary" id="addClientBtn">
                            <i class="fa-solid fa-user-plus"></i>
                            <span>Add Client</span>
                        </button>
                    </div>

                    <h3>Client List (Syncing...)</h3>
                    <ul id="clientList" class="sync-list"></ul>
                </section>
            </main>
        </div>
    </div>

    <script>
        const STORAGE_KEY = 'studentData';
        const NOTIFICATION_KEY = 'fastAssignmentNotifications';
        const TASK_ID_PREFIX = 'FST';
        const TASK_ID_START = 1000;
        const LOGIN_USERNAME = 'Admin';
        const LOGIN_PASSWORD = '638455';
        const LIMITED_USERNAME = 'Dilmi';
        const LIMITED_PASSWORD = '1234';
        const firebaseConfig = {
            apiKey: "AIzaSyDRI1LlKrt3v4KySL7Cnr_WkS3tcWQjrg4",
            authDomain: "fastassig-c57c1.firebaseapp.com",
            projectId: "fastassig-c57c1",
            storageBucket: "fastassig-c57c1.appspot.com",
            messagingSenderId: "1004167924025",
            appId: "1:1004167924025:web:079969223ba8898b9800e8",
            measurementId: "G-0XKNS0YDZH"
        };

        const elements = {
            authScreen: document.getElementById('authScreen'),
            appShell: document.getElementById('appShell'),
            loginForm: document.getElementById('loginForm'),
            loginUsername: document.getElementById('loginUsername'),
            loginPassword: document.getElementById('loginPassword'),
            loginError: document.getElementById('loginError'),
            roleBadge: document.getElementById('roleBadge'),
            logoutBtn: document.getElementById('logoutBtn'),
            notificationBtn: document.getElementById('notificationBtn'),
            notificationCount: document.getElementById('notificationCount'),
            notificationPanel: document.getElementById('notificationPanel'),
            notificationList: document.getElementById('notificationList'),
            clearNotificationsBtn: document.getElementById('clearNotificationsBtn'),
            clientInput: document.getElementById('clientInput'),
            addClientBtn: document.getElementById('addClientBtn'),
            clientList: document.getElementById('clientList'),
            name: document.getElementById('name'),
            date: document.getElementById('date'),
            type: document.getElementById('type'),
            full: document.getElementById('full'),
            adv: document.getElementById('adv'),
            bal: document.getElementById('bal'),
            amountFields: [
                document.getElementById('full').closest('.field'),
                document.getElementById('adv').closest('.field'),
                document.getElementById('bal').closest('.field')
            ],
            saveBtn: document.getElementById('saveBtn'),
            reportBtn: document.getElementById('reportBtn'),
            tableBody: document.getElementById('tableBody'),
            emptyState: document.getElementById('emptyState'),
            emptyStateText: document.getElementById('emptyStateText'),
            totalTasks: document.getElementById('totalTasks'),
            completedTasks: document.getElementById('completedTasks'),
            pendingPayments: document.getElementById('pendingPayments'),
            activeTabBtn: document.getElementById('activeTabBtn'),
            pendingTabBtn: document.getElementById('pendingTabBtn'),
            completedTabBtn: document.getElementById('completedTabBtn'),
            tableTitle: document.getElementById('tableTitle'),
            tableSubtitle: document.getElementById('tableSubtitle'),
            taskTable: document.getElementById('taskTable'),
            financeContractValue: document.getElementById('financeContractValue'),
            financeReceivedValue: document.getElementById('financeReceivedValue'),
            financeReceivableValue: document.getElementById('financeReceivableValue'),
            financeContractBar: document.getElementById('financeContractBar'),
            financeReceivedBar: document.getElementById('financeReceivedBar'),
            financeReceivableBar: document.getElementById('financeReceivableBar'),
            financeContractBarLabel: document.getElementById('financeContractBarLabel'),
            financeReceivedBarLabel: document.getElementById('financeReceivedBarLabel'),
            financeReceivableBarLabel: document.getElementById('financeReceivableBarLabel'),
            bottomFinanceSection: document.getElementById('bottomFinanceSection'),
            reportSection: document.getElementById('reportSection'),
            closeReportBtn: document.getElementById('closeReportBtn'),
            reportGeneratedAt: document.getElementById('reportGeneratedAt'),
            reportTotalTasks: document.getElementById('reportTotalTasks'),
            reportActiveTasks: document.getElementById('reportActiveTasks'),
            reportPendingTasks: document.getElementById('reportPendingTasks'),
            reportCompletedTasks: document.getElementById('reportCompletedTasks'),
            reportContractValue: document.getElementById('reportContractValue'),
            reportReceivedValue: document.getElementById('reportReceivedValue'),
            reportReceivableValue: document.getElementById('reportReceivableValue'),
            reportTableBody: document.getElementById('reportTableBody')
        };

        let currentTab = 'active';
        let currentUserRole = null;
        let firebaseDatabase = null;

        function initializeFirebaseSync() {
            if (typeof firebase === 'undefined') {
                console.error('Firebase SDK did not load.');
                return;
            }

            if (!firebase.apps.length) {
                firebase.initializeApp(firebaseConfig);
            }

            firebaseDatabase = firebase.database();
            firebaseDatabase.ref('clients/').on('value', (snapshot) => {
                elements.clientList.innerHTML = '';

                snapshot.forEach((childSnapshot) => {
                    const data = childSnapshot.val() || {};
                    const li = document.createElement('li');
                    li.textContent = `${data.name || 'Unnamed Client'} (Added on: ${data.time || 'Unknown'})`;
                    elements.clientList.appendChild(li);
                });
            });
        }

        function addClient() {
            if (!firebaseDatabase) {
                alert('Firebase is not connected yet.');
                return;
            }

            const clientName = elements.clientInput.value.trim();

            if (clientName !== "") {
                firebaseDatabase.ref('clients/').push({
                    name: clientName,
                    time: new Date().toLocaleString()
                });
                elements.clientInput.value = '';
                alert('Client added to Cloud!');
            } else {
                alert('Please enter a name!');
            }
        }

        function canViewAmounts() {
            return currentUserRole === 'admin';
        }

        function getNotifications() {
            try {
                const raw = localStorage.getItem(NOTIFICATION_KEY);
                const items = raw ? JSON.parse(raw) : [];
                return Array.isArray(items) ? items : [];
            } catch (error) {
                console.error('Unable to load notifications:', error);
                return [];
            }
        }

        function setNotifications(items) {
            localStorage.setItem(NOTIFICATION_KEY, JSON.stringify(items));
        }

        function addAdminNotification(message) {
            const items = getNotifications();
            items.unshift({
                id: `${Date.now()}`,
                createdAt: new Date().toLocaleString(),
                message
            });
            setNotifications(items.slice(0, 20));
        }

        function renderNotifications() {
            if (currentUserRole !== 'admin') {
                elements.notificationBtn.classList.add('hidden');
                elements.notificationPanel.classList.add('hidden');
                elements.notificationList.innerHTML = '';
                return;
            }

            const items = getNotifications();
            elements.notificationBtn.classList.remove('hidden');
            elements.notificationCount.textContent = String(items.length);
            elements.notificationPanel.classList.toggle('hidden', items.length === 0);
            elements.notificationList.innerHTML = items.map((item) => `
                <div class="notice-item">
                    <strong>${escapeHtml(item.createdAt)}</strong><br>
                    ${escapeHtml(item.message)}
                </div>
            `).join('');
        }

        function applyRoleVisibility() {
            const showAmounts = canViewAmounts();

            elements.amountFields.forEach((field) => {
                field.classList.toggle('hidden', !showAmounts);
            });

            elements.reportBtn.classList.toggle('hidden', !showAmounts);
            elements.bottomFinanceSection.classList.toggle('hidden', !showAmounts);
            elements.pendingTabBtn.classList.toggle('hidden', !showAmounts);
            elements.reportSection.classList.toggle('hidden', !showAmounts);

            if (!showAmounts && currentTab === 'pending') {
                currentTab = 'active';
            }

            elements.roleBadge.textContent = showAmounts ? 'Admin Dashboard' : 'Dilmi Workspace';
            renderNotifications();
        }

        function showApp() {
            elements.authScreen.classList.add('hidden');
            elements.appShell.classList.remove('hidden');
        }

        function logout() {
            currentUserRole = null;
            currentTab = 'active';
            elements.appShell.classList.add('hidden');
            elements.authScreen.classList.remove('hidden');
            elements.loginForm.reset();
            elements.loginError.textContent = '';
            elements.notificationPanel.classList.add('hidden');
            elements.notificationList.innerHTML = '';
            elements.loginUsername.focus();
        }

        function handleLogin(event) {
            event.preventDefault();

            const username = elements.loginUsername.value.trim();
            const password = elements.loginPassword.value;

            if (username === LOGIN_USERNAME && password === LOGIN_PASSWORD) {
                currentUserRole = 'admin';
                elements.loginError.textContent = '';
                applyRoleVisibility();
                showApp();
                renderTasks();
                return;
            }

            if (username === LIMITED_USERNAME && password === LIMITED_PASSWORD) {
                currentUserRole = 'limited';
                elements.loginError.textContent = '';
                applyRoleVisibility();
                showApp();
                renderTasks();
                return;
            }

            elements.loginError.textContent = 'Invalid username or password.';
            elements.loginPassword.value = '';
            elements.loginPassword.focus();
        }

        function toNumber(value) {
            const parsed = parseFloat(value);
            return Number.isFinite(parsed) ? parsed : 0;
        }

        function formatMoney(value) {
            return toNumber(value).toFixed(2);
        }

        function escapeHtml(value) {
            return String(value).replace(/[&<>"']/g, (char) => ({
                '&': '&amp;',
                '<': '&lt;',
                '>': '&gt;',
                '"': '&quot;',
                "'": '&#39;'
            }[char]));
        }

        function normalizeDateInput(dateValue) {
            if (!dateValue) {
                return '';
            }
            return new Date(`${dateValue}T00:00:00`);
        }

        function getTodayAtMidnight() {
            const now = new Date();
            now.setHours(0, 0, 0, 0);
            return now;
        }

        function daysBetween(targetDate, baseDate) {
            const millisecondsPerDay = 24 * 60 * 60 * 1000;
            return Math.ceil((targetDate - baseDate) / millisecondsPerDay);
        }

        function getDueDateStatus(dateValue) {
            if (!dateValue) {
                return { rowClass: '', dateClass: 'date-normal', label: '-' };
            }

            const date = normalizeDateInput(dateValue);
            const today = getTodayAtMidnight();
            const diffDays = daysBetween(date, today);

            if (diffDays < 0) {
                return { rowClass: 'row-overdue', dateClass: 'date-overdue', label: dateValue };
            }

            if (diffDays <= 7) {
                return { rowClass: 'row-urgent', dateClass: 'date-urgent', label: dateValue };
            }

            return { rowClass: '', dateClass: 'date-normal', label: dateValue };
        }

        function sortTasksByDate(tasks) {
            return [...tasks].sort((left, right) => {
                const leftHasDate = Boolean(left.date);
                const rightHasDate = Boolean(right.date);

                if (!leftHasDate && !rightHasDate) {
                    return String(left.id).localeCompare(String(right.id));
                }

                if (!leftHasDate) {
                    return 1;
                }

                if (!rightHasDate) {
                    return -1;
                }

                const leftTime = normalizeDateInput(left.date).getTime();
                const rightTime = normalizeDateInput(right.date).getTime();

                if (leftTime !== rightTime) {
                    return leftTime - rightTime;
                }

                return String(left.id).localeCompare(String(right.id));
            });
        }

        function legacyDateFromId(value) {
            if (typeof value !== 'number' || !Number.isFinite(value)) {
                return '';
            }

            const date = new Date(value);
            if (Number.isNaN(date.getTime())) {
                return '';
            }

            return date.toISOString().slice(0, 10);
        }

        function normalizeTask(task) {
            if (!task || typeof task !== 'object') {
                return null;
            }

            const normalizedDate = task.date || legacyDateFromId(task.id);
            const idValue = typeof task.id === 'string' && task.id.trim()
                ? task.id.trim()
                : typeof task.id === 'number' && Number.isFinite(task.id)
                    ? `legacy-${task.id}`
                    : '';

            return {
                id: idValue,
                name: String(task.name || '').trim(),
                date: normalizedDate,
                type: task.type === 'Project' ? 'Project' : 'Assignment',
                full: formatMoney(task.full),
                adv: formatMoney(task.adv),
                bal: formatMoney(task.bal),
                status: ['completed', 'pending-payment', 'active'].includes(task.status) ? task.status : 'active'
            };
        }

        function getTasks() {
            try {
                const raw = localStorage.getItem(STORAGE_KEY);
                const tasks = raw ? JSON.parse(raw) : [];
                if (!Array.isArray(tasks)) {
                    return [];
                }

                return tasks
                    .map(normalizeTask)
                    .filter((task) => task && task.name);
            } catch (error) {
                console.error('Unable to load tasks:', error);
                return [];
            }
        }

        function setTasks(tasks) {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
        }

        function generateTaskId(taskDate, existingTasks) {
            const nextNumber = existingTasks.reduce((maxValue, task) => {
                const idText = String(task.id || '');
                const match = idText.match(/^FST(\d+)$/i);
                if (!match) {
                    return maxValue;
                }

                return Math.max(maxValue, Number(match[1]));
            }, TASK_ID_START - 1) + 1;

            return `${TASK_ID_PREFIX}${nextNumber}`;
        }

        function doMath() {
            const full = toNumber(elements.full.value);
            const advance = toNumber(elements.adv.value);
            const balance = Math.max(full - advance, 0);
            elements.bal.value = formatMoney(balance);
        }

        function clearInputs() {
            elements.name.value = '';
            elements.date.value = '';
            elements.type.selectedIndex = 0;
            elements.full.value = '';
            elements.adv.value = '';
            elements.bal.value = '';
        }

        function renderSummary(tasks) {
            const completed = tasks.filter((task) => task.status === 'completed').length;
            const pending = tasks.filter((task) => task.status === 'pending-payment').length;
            const totalContractValue = tasks.reduce((sum, task) => sum + toNumber(task.full), 0);
            const totalReceived = tasks.reduce((sum, task) => sum + toNumber(task.adv), 0);
            const totalReceivable = tasks.reduce((sum, task) => sum + toNumber(task.bal), 0);
            const baseAmount = totalContractValue > 0 ? totalContractValue : 1;
            const contractPercent = totalContractValue > 0 ? 100 : 0;
            const receivedPercent = Math.min((totalReceived / baseAmount) * 100, 100);
            const receivablePercent = Math.min((totalReceivable / baseAmount) * 100, 100);

            elements.totalTasks.textContent = tasks.length;
            elements.financeContractValue.textContent = formatMoney(totalContractValue);
            elements.financeReceivedValue.textContent = formatMoney(totalReceived);
            elements.financeReceivableValue.textContent = formatMoney(totalReceivable);
            elements.financeContractBar.style.width = `${contractPercent}%`;
            elements.financeReceivedBar.style.width = `${receivedPercent}%`;
            elements.financeReceivableBar.style.width = `${receivablePercent}%`;
            elements.financeContractBarLabel.textContent = `${Math.round(contractPercent)}%`;
            elements.financeReceivedBarLabel.textContent = `${Math.round(receivedPercent)}%`;
            elements.financeReceivableBarLabel.textContent = `${Math.round(receivablePercent)}%`;
            elements.completedTasks.textContent = completed;
            elements.pendingPayments.textContent = pending;
        }

        function tableHeaderMarkup() {
            return `
                <tr>
                    <th>Task ID</th>
                    <th>Name</th>
                    <th>Due Date</th>
                    <th>Type</th>
                    ${canViewAmounts() ? '<th>Full</th><th>Advance</th><th>Balance</th>' : ''}
                    <th>Status</th>
                    <th>Action</th>
                </tr>
            `;
        }

        function typeBadge(type) {
            const badgeClass = type === 'Project' ? 'badge-project' : 'badge-assignment';
            return `<span class="badge ${badgeClass}">${escapeHtml(type)}</span>`;
        }

        function balanceBadge(balance) {
            const numeric = toNumber(balance);
            const badgeClass = numeric === 0 ? 'badge-paid' : 'badge-due';
            const label = numeric === 0 ? `Paid ${formatMoney(numeric)}` : `Due ${formatMoney(numeric)}`;
            return `<span class="badge ${badgeClass}">${label}</span>`;
        }

        function statusBadge(status) {
            if (status === 'completed') {
                return '<span class="badge badge-paid">Completed</span>';
            }
            if (status === 'pending-payment') {
                return '<span class="badge badge-due">Pending Payment</span>';
            }
            return '<span class="badge badge-due">Active</span>';
        }

        function setCurrentTab(tab) {
            const allowedTabs = canViewAmounts() ? ['active', 'pending', 'completed'] : ['active', 'completed'];
            currentTab = allowedTabs.includes(tab) ? tab : 'active';
            elements.activeTabBtn.classList.toggle('active', currentTab === 'active');
            elements.pendingTabBtn.classList.toggle('active', currentTab === 'pending');
            elements.completedTabBtn.classList.toggle('active', currentTab === 'completed');
            elements.tableTitle.textContent =
                currentTab === 'active' ? 'Active Tasks' :
                currentTab === 'pending' ? 'Pending Payment List' :
                'Completed List';
            elements.tableSubtitle.textContent =
                currentTab === 'active'
                    ? 'Edit payments, complete tasks, and keep active work organized.'
                    : currentTab === 'pending'
                        ? 'Finished assignments with remaining balance are tracked here until fully paid.'
                        : 'Finished and fully paid tasks are moved here for reference.';
            elements.emptyStateText.textContent =
                currentTab === 'active'
                    ? 'No active tasks right now. Save your first task to populate the dashboard.'
                    : currentTab === 'pending'
                        ? 'No pending payments right now. Finished tasks with due balance will appear here.'
                        : 'No completed tasks yet. Fully paid finished tasks will appear here.';
        }

        function renderTasks() {
            const tasks = getTasks();
            renderSummary(tasks);
            setCurrentTab(currentTab);
            renderNotifications();
            document.querySelector('#taskTable thead').innerHTML = tableHeaderMarkup();

            const visibleTasks = sortTasksByDate(tasks.filter((task) => (
                currentTab === 'completed'
                    ? task.status === 'completed'
                    : currentTab === 'pending'
                        ? task.status === 'pending-payment'
                        : task.status === 'active'
            )));

            if (!visibleTasks.length) {
                elements.tableBody.innerHTML = '';
                elements.emptyState.style.display = 'block';
                return;
            }

            const rows = visibleTasks.map((task) => {
                const dateMeta = getDueDateStatus(task.date);
                const showCompleteButton = task.status === 'active';
                const showAddPaymentButton = canViewAmounts() && (task.status === 'active' || task.status === 'pending-payment');
                const showEditButton = canViewAmounts();
                const showDeleteButton = currentUserRole === 'admin';
                return `
                    <tr class="${dateMeta.rowClass}">
                        <td><strong>${escapeHtml(task.id)}</strong></td>
                        <td>${escapeHtml(task.name)}</td>
                        <td class="${dateMeta.dateClass}">${escapeHtml(dateMeta.label)}</td>
                        <td>${typeBadge(task.type)}</td>
                        ${canViewAmounts() ? `<td class="money">${formatMoney(task.full)}</td>` : ''}
                        ${canViewAmounts() ? `<td class="money">${formatMoney(task.adv)}</td>` : ''}
                        ${canViewAmounts() ? `<td>${balanceBadge(task.bal)}</td>` : ''}
                        <td>${statusBadge(task.status)}</td>
                        <td>
                            <div class="actions-cell">
                                ${showEditButton ? `
                                    <button type="button" class="btn-info" data-action="edit" data-id="${escapeHtml(task.id)}">
                                        <i class="fa-solid fa-pen-to-square"></i>
                                        <span>Edit</span>
                                    </button>
                                ` : ''}
                                ${showAddPaymentButton ? `
                                    <button type="button" class="btn-secondary" data-action="payment" data-id="${escapeHtml(task.id)}">
                                        <i class="fa-solid fa-money-bill-wave"></i>
                                        <span>Add Payment</span>
                                    </button>
                                ` : ''}
                                ${showCompleteButton ? `
                                    <button type="button" class="btn-success" data-action="complete" data-id="${escapeHtml(task.id)}">
                                        <i class="fa-solid fa-circle-check"></i>
                                        <span>Mark Done</span>
                                    </button>
                                ` : ''}
                                ${showDeleteButton ? `
                                    <button type="button" class="btn-danger" data-action="delete" data-id="${escapeHtml(task.id)}">
                                        <i class="fa-solid fa-trash"></i>
                                        <span>Delete</span>
                                    </button>
                                ` : ''}
                            </div>
                        </td>
                    </tr>
                `;
            }).join('');

            elements.tableBody.innerHTML = rows;
            elements.emptyState.style.display = 'none';
        }

        function generateFullReport() {
            if (!canViewAmounts()) {
                return;
            }

            const tasks = sortTasksByDate(getTasks());
            const totalTasks = tasks.length;
            const activeTasks = tasks.filter((task) => task.status === 'active').length;
            const pendingTasks = tasks.filter((task) => task.status === 'pending-payment').length;
            const completedTasks = tasks.filter((task) => task.status === 'completed').length;
            const contractValue = tasks.reduce((sum, task) => sum + toNumber(task.full), 0);
            const receivedValue = tasks.reduce((sum, task) => sum + toNumber(task.adv), 0);
            const receivableValue = tasks.reduce((sum, task) => sum + toNumber(task.bal), 0);

            elements.reportGeneratedAt.textContent = `Generated on ${new Date().toLocaleString()}`;
            elements.reportTotalTasks.textContent = String(totalTasks);
            elements.reportActiveTasks.textContent = String(activeTasks);
            elements.reportPendingTasks.textContent = String(pendingTasks);
            elements.reportCompletedTasks.textContent = String(completedTasks);
            elements.reportContractValue.textContent = formatMoney(contractValue);
            elements.reportReceivedValue.textContent = formatMoney(receivedValue);
            elements.reportReceivableValue.textContent = formatMoney(receivableValue);

            elements.reportTableBody.innerHTML = tasks.map((task) => `
                <tr>
                    <td>${escapeHtml(task.id)}</td>
                    <td>${escapeHtml(task.name)}</td>
                    <td>${escapeHtml(task.date || '-')}</td>
                    <td>${escapeHtml(task.type)}</td>
                    <td>${escapeHtml(task.status)}</td>
                    <td>${formatMoney(task.full)}</td>
                    <td>${formatMoney(task.adv)}</td>
                    <td>${formatMoney(task.bal)}</td>
                </tr>
            `).join('');

            elements.reportSection.classList.remove('hidden');
            elements.reportSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }

        function saveTask() {
            const tasks = getTasks();
            const name = elements.name.value.trim();
            const date = elements.date.value;
            const type = elements.type.value;
            const full = formatMoney(elements.full.value);
            const adv = formatMoney(elements.adv.value);
            const bal = formatMoney(elements.bal.value);

            if (!name) {
                alert('Please enter a client name.');
                elements.name.focus();
                return;
            }

            const task = {
                id: generateTaskId(date, tasks),
                name,
                date,
                type,
                full: canViewAmounts() ? full : '0.00',
                adv: canViewAmounts() ? adv : '0.00',
                bal: canViewAmounts() ? bal : '0.00',
                status: 'active'
            };

            tasks.push(task);
            setTasks(tasks);
            renderTasks();
            clearInputs();
        }

        function editTask(taskId) {
            const tasks = getTasks();
            const task = tasks.find((item) => item.id === taskId);

            if (!task) {
                return;
            }

            const newFullValue = window.prompt(`Update full payment for ${task.name}`, formatMoney(task.full));
            if (newFullValue === null) {
                return;
            }

            const parsedFull = Math.max(toNumber(newFullValue), 0);
            const parsedAdvance = Math.min(toNumber(task.adv), parsedFull);
            const nextBalance = Math.max(parsedFull - parsedAdvance, 0);

            task.full = formatMoney(parsedFull);
            task.adv = formatMoney(parsedAdvance);
            task.bal = formatMoney(nextBalance);

            if (task.status === 'pending-payment' && nextBalance === 0) {
                task.status = 'completed';
            }

            setTasks(tasks);
            if (task.status === 'completed') {
                setCurrentTab('completed');
            } else if (task.status === 'pending-payment') {
                setCurrentTab('pending');
            }
            renderTasks();
        }

        function addPayment(taskId) {
            const tasks = getTasks();
            const task = tasks.find((item) => item.id === taskId);

            if (!task) {
                return;
            }

            const suggestedAmount = toNumber(task.bal) > 0 ? formatMoney(task.bal) : '0.00';
            const paymentInput = window.prompt(`Add received payment for ${task.name}`, suggestedAmount);
            if (paymentInput === null) {
                return;
            }

            const receivedAmount = Math.max(toNumber(paymentInput), 0);
            const currentAdvance = toNumber(task.adv);
            const currentFull = toNumber(task.full);
            const newAdvance = Math.min(currentAdvance + receivedAmount, currentFull);
            const newBalance = Math.max(currentFull - newAdvance, 0);

            task.adv = formatMoney(newAdvance);
            task.bal = formatMoney(newBalance);

            if (task.status === 'pending-payment') {
                task.status = newBalance === 0 ? 'completed' : 'pending-payment';
            }

            setTasks(tasks);
            if (task.status === 'completed') {
                setCurrentTab('completed');
            } else if (task.status === 'pending-payment') {
                setCurrentTab('pending');
            }
            renderTasks();
        }

        function completeTask(taskId) {
            const tasks = getTasks();
            const task = tasks.find((item) => item.id === taskId);

            if (!task) {
                return;
            }

            const balance = toNumber(task.bal);
            task.status = balance > 0 ? 'pending-payment' : 'completed';

            if (currentUserRole === 'limited') {
                addAdminNotification(`Dilmi marked task ${task.id} (${task.name}) as done.`);
            }

            setTasks(tasks);
            if (task.status === 'pending-payment') {
                setCurrentTab('pending');
            } else if (currentTab === 'active') {
                setCurrentTab('completed');
            }
            renderTasks();
        }

        function deleteTask(taskId) {
            const tasks = getTasks().filter((task) => task.id !== taskId);
            setTasks(tasks);
            renderTasks();
        }

        elements.full.addEventListener('input', doMath);
        elements.adv.addEventListener('input', doMath);
        elements.saveBtn.addEventListener('click', saveTask);
        elements.reportBtn.addEventListener('click', generateFullReport);
        elements.closeReportBtn.addEventListener('click', () => {
            elements.reportSection.classList.add('hidden');
        });
        elements.loginForm.addEventListener('submit', handleLogin);
        elements.logoutBtn.addEventListener('click', logout);
        elements.notificationBtn.addEventListener('click', () => {
            elements.notificationPanel.classList.toggle('hidden');
        });
        elements.clearNotificationsBtn.addEventListener('click', () => {
            setNotifications([]);
            renderNotifications();
        });
        elements.addClientBtn.addEventListener('click', addClient);
        elements.activeTabBtn.addEventListener('click', () => {
            setCurrentTab('active');
            renderTasks();
        });
        elements.pendingTabBtn.addEventListener('click', () => {
            if (!canViewAmounts()) {
                return;
            }
            setCurrentTab('pending');
            renderTasks();
        });
        elements.completedTabBtn.addEventListener('click', () => {
            setCurrentTab('completed');
            renderTasks();
        });

        elements.tableBody.addEventListener('click', (event) => {
            const button = event.target.closest('[data-id][data-action]');
            if (!button) {
                return;
            }

            const { action, id } = button.dataset;

            if (action === 'edit') {
                editTask(id);
                return;
            }

            if (action === 'payment') {
                addPayment(id);
                return;
            }

            if (action === 'complete') {
                completeTask(id);
                return;
            }

            if (action === 'delete') {
                deleteTask(id);
            }
        });

        window.addEventListener('load', () => {
            initializeFirebaseSync();
            elements.loginUsername.focus();
        });
    </script>
</body>
</html>
