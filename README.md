<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="艾宾浩斯学习">
    <meta name="theme-color" content="#6366f1">
    <meta name="description" content="基于艾宾浩斯遗忘曲线的科学学习计划工具，含积分奖励与惩罚机制">
    <title>🧠 艾宾浩斯学习计划</title>
    <link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Ccircle cx='50' cy='50' r='48' fill='%236366f1'/%3E%3Ctext x='50' y='64' text-anchor='middle' font-size='40'%3E🧠%3C/text%3E%3C/svg%3E">
    <style>
        :root {
            --bg: #f1f5f9;
            --card: #ffffff;
            --text: #1e293b;
            --text-secondary: #64748b;
            --text-muted: #94a3b8;
            --border: #e2e8f0;
            --primary: #6366f1;
            --primary-light: #a5b4fc;
            --primary-bg: #eef2ff;
            --success: #10b981;
            --success-bg: #ecfdf5;
            --warning: #f59e0b;
            --warning-bg: #fffbeb;
            --danger: #ef4444;
            --danger-bg: #fef2f2;
            --info: #3b82f6;
            --info-bg: #eff6ff;
            --gold: #f59e0b;
            --gold-bg: #fff7ed;
            --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.06);
            --shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
            --shadow-lg: 0 12px 36px rgba(0, 0, 0, 0.14);
            --radius: 10px;
            --radius-lg: 14px;
            --radius-xl: 18px;
            --radius-2xl: 22px;
            --transition: 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            --nav-height: 60px;
            --safe-bottom: env(safe-area-inset-bottom, 10px);
        }
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        html {
            scroll-behavior: smooth;
            -webkit-overflow-scrolling: touch;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif;
            background: #e8ecf1;
            color: var(--text);
            min-height: 100vh;
            min-height: -webkit-fill-available;
            display: flex;
            justify-content: center;
            padding: 8px 8px calc(var(--nav-height) + var(--safe-bottom) + 16px);
            -webkit-tap-highlight-color: transparent;
            -webkit-user-select: none;
            user-select: none;
            -webkit-font-smoothing: antialiased;
        }
        .app {
            width: 100%;
            max-width: 960px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .header {
            background: linear-gradient(135deg, #6366f1 0%, #8b5cf6 40%, #a855f7 100%);
            border-radius: var(--radius-2xl);
            padding: 14px 16px;
            color: #fff;
            box-shadow: 0 8px 28px rgba(99, 102, 241, 0.32);
            position: relative;
            overflow: hidden;
            cursor: default;
        }
        .header::after {
            content: '';
            position: absolute;
            top: -50px;
            right: -40px;
            width: 180px;
            height: 180px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 50%;
            pointer-events: none;
        }
        .header-top {
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 8px;
            position: relative;
            z-index: 1;
        }
        .header-title {
            font-size: 17px;
            font-weight: 700;
            letter-spacing: -0.3px;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .header-points {
            display: flex;
            align-items: center;
            gap: 4px;
            background: rgba(255, 255, 255, 0.18);
            border-radius: 16px;
            padding: 5px 12px;
            font-weight: 700;
            font-size: 13px;
            letter-spacing: -0.2px;
        }
        .header-points .coin {
            font-size: 16px;
        }
        .header-sub {
            font-size: 10px;
            opacity: 0.85;
            margin-top: 4px;
            position: relative;
            z-index: 1;
            line-height: 1.4;
        }
        .header-badges {
            display: flex;
            gap: 5px;
            flex-wrap: wrap;
            margin-top: 6px;
            position: relative;
            z-index: 1;
        }
        .header-badge {
            display: inline-flex;
            align-items: center;
            gap: 3px;
            background: rgba(255, 255, 255, 0.18);
            border-radius: 14px;
            padding: 4px 9px;
            font-size: 9px;
            font-weight: 500;
            letter-spacing: 0.2px;
        }
        .header-badge.warn {
            background: rgba(255, 180, 50, 0.3);
            animation: pulse-badge 2s infinite;
        }
        .header-badge.level-badge {
            background: rgba(255, 215, 0, 0.3);
            font-weight: 700;
        }
        @keyframes pulse-badge {
            0%,
            100% {
                opacity: 1;
            }
            50% {
                opacity: 0.6;
            }
        }

        .install-banner {
            background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
            border-radius: var(--radius-lg);
            padding: 10px 14px;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 11px;
            font-weight: 500;
            color: #fff;
            box-shadow: var(--shadow);
            cursor: pointer;
            transition: all var(--transition);
            animation: slideDown 0.4s ease;
        }
        @keyframes slideDown {
            from {
                opacity: 0;
                transform: translateY(-14px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        .install-banner:active {
            transform: scale(0.97);
        }
        .install-banner .install-close {
            margin-left: auto;
            background: rgba(255, 255, 255, 0.15);
            border: none;
            color: #fff;
            width: 22px;
            height: 22px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 12px;
            flex-shrink: 0;
        }

        .reminder-banner {
            background: linear-gradient(135deg, #fef3c7 0%, #fde68a 100%);
            border-radius: var(--radius-lg);
            padding: 10px 14px;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 11px;
            font-weight: 600;
            color: #92400e;
            box-shadow: var(--shadow-sm);
            cursor: pointer;
            transition: all var(--transition);
            border: 1.5px solid #fcd34d;
            animation: slideDown 0.35s ease;
        }
        .reminder-banner:active {
            transform: scale(0.98);
        }
        .reminder-banner .reminder-close {
            margin-left: auto;
            width: 22px;
            height: 22px;
            border-radius: 50%;
            border: none;
            background: rgba(0, 0, 0, 0.08);
            cursor: pointer;
            font-size: 13px;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .penalty-banner {
            background: linear-gradient(135deg, #fef2f2 0%, #fecaca 100%);
            border-radius: var(--radius-lg);
            padding: 10px 14px;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 11px;
            font-weight: 600;
            color: #991b1b;
            border: 1.5px solid #fca5a5;
            animation: shake 0.5s ease;
        }
        @keyframes shake {
            0%,
            100% {
                transform: translateX(0);
            }
            25% {
                transform: translateX(-6px);
            }
            75% {
                transform: translateX(6px);
            }
        }

        .stats-row {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
            gap: 7px;
        }
        .stat-card {
            background: var(--card);
            border-radius: var(--radius-lg);
            padding: 11px 8px;
            text-align: center;
            box-shadow: var(--shadow-sm);
            transition: all var(--transition);
            cursor: default;
            position: relative;
        }
        .stat-card:active {
            transform: scale(0.95);
            box-shadow: var(--shadow);
        }
        .stat-card .val {
            font-size: 22px;
            font-weight: 700;
            line-height: 1;
            letter-spacing: -1px;
        }
        .stat-card .lbl {
            font-size: 8px;
            color: var(--text-muted);
            margin-top: 3px;
            text-transform: uppercase;
            letter-spacing: 0.3px;
            font-weight: 500;
        }
        .stat-card.accent {
            border-left: 3px solid var(--primary);
        }
        .stat-card.gold {
            border-left: 3px solid var(--gold);
        }

        .main-grid {
            display: grid;
            grid-template-columns: 1fr 1.5fr;
            gap: 10px;
        }
        @media (max-width: 720px) {
            .main-grid {
                grid-template-columns: 1fr;
            }
            .header-title {
                font-size: 15px;
            }
            .header {
                padding: 12px 13px;
            }
            .stat-card .val {
                font-size: 18px;
            }
        }

        .card {
            background: var(--card);
            border-radius: var(--radius-lg);
            padding: 13px 15px;
            box-shadow: var(--shadow-sm);
            transition: box-shadow var(--transition);
        }
        .card-title {
            font-weight: 700;
            font-size: 12px;
            display: flex;
            align-items: center;
            gap: 5px;
            margin-bottom: 8px;
            letter-spacing: -0.2px;
        }
        .card-subtitle {
            font-size: 9px;
            color: var(--text-muted);
            margin-bottom: 6px;
            line-height: 1.4;
        }

        .form-row {
            display: flex;
            gap: 5px;
            flex-wrap: wrap;
            margin-bottom: 6px;
        }
        .form-row input,
        .form-row select {
            font-family: inherit;
            padding: 9px 12px;
            border-radius: 20px;
            border: 2px solid var(--border);
            font-size: 11px;
            background: #fafbfc;
            outline: none;
            transition: all var(--transition);
            flex: 1;
            min-width: 60px;
            -webkit-appearance: none;
            appearance: none;
        }
        .form-row input:focus,
        .form-row select:focus {
            border-color: var(--primary-light);
            background: #fff;
            box-shadow: 0 0 0 4px rgba(99, 102, 241, 0.06);
        }
        .form-row select {
            cursor: pointer;
            padding-right: 26px;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='10' viewBox='0 0 24 24' fill='none' stroke='%2364748b' stroke-width='2.5'%3E%3Cpath d='M6 9l6 6 6-6'%3E%3C/path%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 8px center;
        }
        .btn {
            padding: 9px 16px;
            border-radius: 20px;
            border: none;
            cursor: pointer;
            font-size: 11px;
            font-weight: 600;
            font-family: inherit;
            transition: all var(--transition);
            white-space: nowrap;
            letter-spacing: 0.1px;
            -webkit-tap-highlight-color: transparent;
            touch-action: manipulation;
        }
        .btn:active {
            transform: scale(0.93);
            transition: transform 0.08s;
        }
        .btn.primary {
            background: var(--primary);
            color: #fff;
            box-shadow: 0 3px 12px rgba(99, 102, 241, 0.3);
        }
        .btn.secondary {
            background: #f1f5f9;
            color: var(--text);
        }
        .btn.danger {
            background: var(--danger-bg);
            color: #dc2626;
        }
        .btn.warn {
            background: #fef3c7;
            color: #b45309;
        }
        .btn.xs {
            padding: 3px 8px;
            font-size: 9px;
            border-radius: 11px;
        }
        .btn.sm {
            padding: 6px 12px;
            font-size: 10px;
            border-radius: 16px;
        }
        .btn-full {
            width: 100%;
        }

        .item-list {
            display: flex;
            flex-direction: column;
            gap: 4px;
            max-height: 260px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }
        .item-row {
            display: flex;
            align-items: center;
            gap: 7px;
            padding: 9px 10px;
            background: #fafbfc;
            border-radius: var(--radius);
            font-size: 10px;
            transition: all var(--transition);
            border: 1.5px solid transparent;
            flex-wrap: wrap;
        }
        .item-row:active {
            border-color: #e0e7ff;
            background: #fff;
            transform: scale(0.97);
        }
        .item-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            flex-shrink: 0;
        }
        .item-dot.learn {
            background: #6366f1;
        }
        .item-dot.review {
            background: #10b981;
        }
        .item-info {
            flex: 1;
            min-width: 60px;
        }
        .item-name {
            font-weight: 600;
            font-size: 11px;
            letter-spacing: -0.1px;
        }
        .item-meta {
            font-size: 8px;
            color: var(--text-muted);
            margin-top: 1px;
        }
        .item-progress {
            font-size: 9px;
            font-weight: 600;
            color: var(--primary);
        }
        .item-progress.done {
            color: var(--success);
        }

        .curve-chart {
            display: flex;
            align-items: flex-end;
            gap: 3px;
            height: 90px;
            padding: 3px 0;
        }
        .curve-chart .bar-group {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 1px;
            min-width: 18px;
        }
        .curve-chart .bar {
            width: 60%;
            border-radius: 3px 3px 0 0;
            transition: height 0.5s ease;
            background: linear-gradient(180deg, #6366f1 0%, #a5b4fc 100%);
            min-width: 5px;
        }
        .curve-chart .bar.reviewed {
            background: linear-gradient(180deg, #10b981 0%, #6ee7b7 100%);
        }
        .curve-chart .bar-label {
            font-size: 6px;
            color: var(--text-muted);
            text-align: center;
            white-space: nowrap;
        }
        .curve-chart .bar-pct {
            font-size: 6px;
            font-weight: 600;
            color: var(--text-secondary);
        }

        .cal-nav {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 4px;
            margin-bottom: 6px;
            flex-wrap: wrap;
        }
        .cal-nav .month-label {
            font-weight: 700;
            font-size: 13px;
            min-width: 80px;
            text-align: center;
        }
        .cal-nav button {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            border: 2px solid var(--border);
            background: #fff;
            cursor: pointer;
            font-size: 13px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all var(--transition);
            color: var(--text);
        }
        .cal-nav button:active {
            background: #eef2ff;
            transform: scale(0.88);
        }
        .cal-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 2px;
            text-align: center;
            font-size: 9px;
        }
        .cal-day-header {
            font-weight: 700;
            color: var(--text-muted);
            padding: 4px 0;
            font-size: 8px;
        }
        .cal-cell {
            aspect-ratio: 1;
            border-radius: 7px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all var(--transition);
            position: relative;
            font-weight: 500;
            font-size: 10px;
            border: 1.5px solid transparent;
            background: #fafbfc;
            color: var(--text);
            min-width: 24px;
            min-height: 24px;
        }
        .cal-cell:active {
            background: #eef2ff;
            border-color: #c7d2fe;
            transform: scale(0.88);
        }
        .cal-cell.other-month {
            color: #d0d5dd;
            background: transparent;
            pointer-events: none;
        }
        .cal-cell.today {
            border: 2.5px solid var(--primary) !important;
            font-weight: 700;
            background: var(--primary-bg);
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.07);
        }
        .cal-cell.has-tasks {
            background: #fff;
            font-weight: 600;
        }
        .cal-cell .task-dots {
            display: flex;
            gap: 2px;
            margin-top: 1px;
        }
        .cal-cell .task-dot {
            width: 4px;
            height: 4px;
            border-radius: 50%;
        }
        .cal-cell .task-dot.learn {
            background: #6366f1;
        }
        .cal-cell .task-dot.review {
            background: #10b981;
        }
        .cal-cell .task-count {
            font-size: 6px;
            font-weight: 700;
            color: #fff;
            position: absolute;
            top: 2px;
            right: 3px;
            background: #6366f1;
            border-radius: 6px;
            padding: 1px 4px;
            line-height: 1;
        }

        .today-task-list {
            display: flex;
            flex-direction: column;
            gap: 3px;
            max-height: 220px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }
        .today-task-item {
            display: flex;
            align-items: center;
            gap: 7px;
            padding: 8px 10px;
            border-radius: 8px;
            font-size: 10px;
            transition: all var(--transition);
            border: 1.5px solid var(--border);
            cursor: pointer;
            background: #fafbfc;
        }
        .today-task-item:active {
            background: #f0f4ff;
            transform: scale(0.96);
        }
        .today-task-item.completed {
            opacity: 0.5;
            text-decoration: line-through;
            background: #f8fafc;
            border-color: #e2e8f0;
        }
        .today-task-item .check-circle {
            width: 18px;
            height: 18px;
            border-radius: 50%;
            border: 2.5px solid #d0d5dd;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 9px;
            color: transparent;
            transition: all var(--transition);
            font-weight: 700;
        }
        .today-task-item.completed .check-circle {
            background: var(--success);
            border-color: var(--success);
            color: #fff;
        }
        .task-type-badge {
            font-size: 7px;
            padding: 2px 6px;
            border-radius: 7px;
            font-weight: 600;
            white-space: nowrap;
        }
        .task-type-badge.new {
            background: #eef2ff;
            color: #6366f1;
        }
        .task-type-badge.review {
            background: #ecfdf5;
            color: #059669;
        }

        .achievement-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
        }
        .achievement-chip {
            display: inline-flex;
            align-items: center;
            gap: 3px;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 9px;
            font-weight: 600;
            background: #f1f5f9;
            border: 1px solid var(--border);
            transition: all var(--transition);
        }
        .achievement-chip.earned {
            background: #fef3c7;
            border-color: #fcd34d;
            animation: popIn 0.4s ease;
        }
        @keyframes popIn {
            0% {
                transform: scale(0);
                opacity: 0;
            }
            70% {
                transform: scale(1.2);
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }

        .settings-section {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }
        .setting-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 8px;
            padding: 6px 0;
            border-bottom: 1px solid var(--border);
            flex-wrap: wrap;
        }
        .setting-row:last-child {
            border-bottom: none;
        }
        .setting-label {
            font-size: 10px;
            font-weight: 600;
            letter-spacing: -0.1px;
        }
        .setting-sublabel {
            font-size: 8px;
            color: var(--text-muted);
        }
        .toggle-switch {
            position: relative;
            width: 42px;
            height: 24px;
            flex-shrink: 0;
        }
        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        .toggle-slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: #d0d5dd;
            border-radius: 24px;
            transition: all 0.3s;
        }
        .toggle-slider::before {
            content: '';
            position: absolute;
            height: 18px;
            width: 18px;
            left: 3px;
            bottom: 3px;
            background: #fff;
            border-radius: 50%;
            transition: all 0.3s;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.15);
        }
        .toggle-switch input:checked+.toggle-slider {
            background: var(--primary);
        }
        .toggle-switch input:checked+.toggle-slider::before {
            transform: translateX(18px);
        }

        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: var(--card);
            border-top: 1.5px solid var(--border);
            display: flex;
            justify-content: space-around;
            align-items: center;
            height: var(--nav-height);
            padding-bottom: var(--safe-bottom);
            z-index: 100;
            box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.06);
        }
        .bottom-nav .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 2px;
            cursor: pointer;
            padding: 5px 10px;
            border-radius: 12px;
            transition: all var(--transition);
            font-size: 8px;
            font-weight: 500;
            color: var(--text-muted);
            border: none;
            background: none;
            font-family: inherit;
            position: relative;
        }
        .bottom-nav .nav-item:active {
            background: #f1f5f9;
            transform: scale(0.88);
        }
        .bottom-nav .nav-item.active {
            color: var(--primary);
            font-weight: 700;
        }
        .bottom-nav .nav-icon {
            font-size: 18px;
        }
        .bottom-nav .nav-badge {
            position: absolute;
            top: 0;
            right: 2px;
            background: #ef4444;
            color: #fff;
            font-size: 7px;
            width: 15px;
            height: 15px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            animation: bounce-badge 0.5s ease;
        }
        @keyframes bounce-badge {
            0%,
            100% {
                transform: scale(1);
            }
            30% {
                transform: scale(1.5);
            }
            60% {
                transform: scale(0.8);
            }
        }
        @media (min-width: 721px) {
            .bottom-nav {
                display: none;
            }
            body {
                padding-bottom: 30px;
            }
        }

        .modal-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.55);
            z-index: 999;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 16px;
            animation: fadeIn 0.2s;
        }
        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }
        .modal {
            background: #fff;
            border-radius: var(--radius-xl);
            padding: 16px;
            width: 100%;
            max-width: 360px;
            box-shadow: var(--shadow-lg);
            max-height: 80vh;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
            animation: modalIn 0.25s ease;
        }
        @keyframes modalIn {
            from {
                opacity: 0;
                transform: translateY(20px) scale(0.95);
            }
            to {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }
        .modal h4 {
            font-size: 13px;
            margin-bottom: 6px;
            font-weight: 700;
        }
        .modal-close-btn {
            position: absolute;
            top: 6px;
            right: 10px;
            width: 26px;
            height: 26px;
            border-radius: 50%;
            border: none;
            background: #f1f5f9;
            cursor: pointer;
            font-size: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1;
        }

        .level-up-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.7);
            z-index: 9999;
            display: flex;
            align-items: center;
            justify-content: center;
            animation: fadeIn 0.2s;
        }
        .level-up-card {
            text-align: center;
            background: linear-gradient(135deg, #fef3c7, #fde68a, #fbbf24);
            border-radius: 24px;
            padding: 24px;
            animation: popIn 0.5s ease;
            box-shadow: 0 20px 60px rgba(245, 158, 11, 0.5);
        }
        .level-up-card .big-emoji {
            font-size: 60px;
            animation: bounce 0.6s ease infinite alternate;
        }
        @keyframes bounce {
            from {
                transform: translateY(0);
            }
            to {
                transform: translateY(-16px);
            }
        }

        .toast-wrap {
            position: fixed;
            top: 16px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 9999;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            gap: 5px;
            align-items: center;
        }
        .toast {
            padding: 9px 16px;
            border-radius: 20px;
            background: #1e293b;
            color: #fff;
            font-size: 11px;
            font-weight: 500;
            animation: toastIn 0.35s, toastOut 0.3s 1.8s forwards;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
            white-space: nowrap;
            max-width: 90vw;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .toast.reward {
            background: #065f46;
            color: #d1fae5;
            font-weight: 700;
        }
        .toast.penalty {
            background: #991b1b;
            color: #fecaca;
            font-weight: 700;
        }
        @keyframes toastIn {
            from {
                opacity: 0;
                transform: translateY(-14px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        @keyframes toastOut {
            from {
                opacity: 1;
            }
            to {
                opacity: 0;
                transform: translateY(-10px);
            }
        }

        ::-webkit-scrollbar {
            width: 3px;
            height: 3px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: #d0d5dd;
            border-radius: 3px;
        }

        @media (prefers-color-scheme: dark) {
            :root {
                --bg: #0f172a;
                --card: #1e293b;
                --text: #e2e8f0;
                --text-secondary: #94a3b8;
                --text-muted: #64748b;
                --border: #334155;
                --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.35);
                --shadow: 0 4px 20px rgba(0, 0, 0, 0.45);
                --primary-bg: #1e2a4a;
                --success-bg: #0f2f24;
                --danger-bg: #3b1515;
                --warning-bg: #3b2f10;
                --gold-bg: #3b2f10;
            }
            body {
                background: #0a0f1a;
            }
            input,
            select {
                background: #1a2332 !important;
                color: #e2e8f0 !important;
                border-color: #334155 !important;
            }
            .item-row {
                background: #1a2332;
            }
            .cal-cell {
                background: #1a2332;
            }
            .cal-cell.has-tasks {
                background: #1e293b;
            }
            .btn.secondary {
                background: #334155;
                color: #e2e8f0;
            }
            .today-task-item {
                background: #1a2332;
                border-color: #334155;
            }
            .modal {
                background: #1e293b;
            }
            .modal .day-task-row {
                background: #1a2332;
            }
            .bottom-nav {
                background: #1e293b;
                border-color: #334155;
            }
            .achievement-chip {
                background: #1e293b;
                border-color: #334155;
            }
            .achievement-chip.earned {
                background: #3b2f10;
            }
            .toggle-slider {
                background: #475569;
            }
            .penalty-banner {
                background: linear-gradient(135deg, #3b1515 0%, #4a1a1a 100%);
                color: #fca5a5;
                border-color: #991b1b;
            }
        }
    </style>
</head>
<body>
    <div class="app" id="app"></div>
    <div class="toast-wrap" id="toastWrap"></div>
    <nav class="bottom-nav" id="bottomNav">
        <button class="nav-item active" data-section="today"> <span class="nav-icon">📋</span>今日 </button>
        <button class="nav-item" data-section="calendar"> <span class="nav-icon">📅</span>日历 </button>
        <button class="nav-item" data-section="items"> <span class="nav-icon">📚</span>条目 </button>
        <button class="nav-item" data-section="settings"> <span class="nav-icon">⚙️</span>设置 </button>
    </nav>
    <script>
        (function() {
            const REVIEW_INTERVALS = [
                { num: 1, days: 1, label: '第1次复习 (1天后)' }, { num: 2, days: 3, label: '第2次复习 (3天后)' },
                { num: 3, days: 7, label: '第3次复习 (7天后)' }, { num: 4, days: 14, label: '第4次复习 (14天后)' },
                { num: 5, days: 30, label: '第5次复习 (30天后)' }, { num: 6, days: 60, label: '第6次复习 (60天后)' },
                { num: 7, days: 90, label: '第7次复习 (90天后)' },
            ];
            const FORGETTING_CURVE = [
                { label: '学习', day: 0, retention: 100 }, { label: '20分', day: 0.01, retention: 58 },
                { label: '1时', day: 0.04, retention: 44 }, { label: '1天', day: 1, retention: 34 },
                { label: '3天', day: 3, retention: 28 }, { label: '7天', day: 7, retention: 25 },
                { label: '14天', day: 14, retention: 21 }, { label: '30天', day: 30, retention: 20 },
                { label: '60天', day: 60, retention: 19 }, { label: '90天', day: 90, retention: 18 },
            ];
            const SUBJECTS = ['📚 英语', '📐 数学', '📖 语文', '🔬 科学', '🏛️ 历史', '🌍 地理', '💻 编程', '🎵 音乐', '🎨 美术', '⚽ 体育',
                '📝 自定义'
            ];

            // 等级系统
            const LEVELS = [
                { name: '🌱 新手', min: 0, icon: '🌱' }, { name: '🌿 初级学习者', min: 100, icon: '🌿' },
                { name: '🌳 进阶学习者', min: 300, icon: '🌳' }, { name: '🎋 勤奋学习者', min: 600, icon: '🎋' },
                { name: '🏆 学霸', min: 1000, icon: '🏆' }, { name: '👑 记忆大师', min: 2000, icon: '👑' },
                { name: '🧠 超级大脑', min: 5000, icon: '🧠' },
            ];

            // 成就定义
            const ACHIEVEMENTS = [
                { id: 'first_learn', name: '初出茅庐', icon: '🥉', desc: '完成第一个学习条目' },
                { id: 'tasks_10', name: '任务达人', icon: '📋', desc: '累计完成10个任务' },
                { id: 'tasks_50', name: '高效执行者', icon: '⚡', desc: '累计完成50个任务' },
                { id: 'tasks_100', name: '百发百中', icon: '🎯', desc: '累计完成100个任务' },
                { id: 'streak_7', name: '坚持一周', icon: '🔥', desc: '连续7天完成每日任务' },
                { id: 'streak_30', name: '月度冠军', icon: '🏅', desc: '连续30天完成每日任务' },
                { id: 'full_mastery_1', name: '完美掌握', icon: '💎', desc: '一个条目完成全部7次复习' },
                { id: 'full_mastery_5', name: '知识渊博', icon: '📖', desc: '5个条目完成全部复习' },
                { id: 'perfect_day', name: '完美一天', icon: '🌟', desc: '一天内完成所有任务' },
                { id: 'comeback', name: '卷土重来', icon: '🦅', desc: '中断后重新连续3天打卡' },
            ];

            const DB_KEY = 'ebbinghaus_v3';
            const REMINDER_KEY = 'ebbinghaus_reminder_v3';
            const LAST_CHECK_KEY = 'ebbinghaus_last_check_v3';
            const LAST_TRIGGER_KEY = 'ebbinghaus_last_trigger_v3';

            let data = {
                items: [],
                completedTasks: {},
                points: 0,
                streak: 0,
                lastActiveDate: null,
                achievements: [],
                totalTasksCompleted: 0,
                dailyCompletions: {},
                fullyMasteredItems: [],
                penaltyCount: 0,
                streakBroken: false,
            };
            let reminderSettings = { enabled: true, times: ['09:00', '20:00'] };
            let lastCheckDate = null;
            let lastTriggered = {};
            let calYear, calMonth;
            let today = new Date();
            today.setHours(0, 0, 0, 0);
            const todayStr = toDateStr(today);
            let currentSection = 'today';
            let deferredInstallPrompt = null;
            let pendingLevelUp = null;

            function toDateStr(d) { return d.toISOString().split('T')[0]; }

            function parseDate(str) { const d = new Date(str + 'T00:00:00');
                d.setHours(0, 0, 0, 0); return d; }

            function addDays(dateStr, days) { const d = parseDate(dateStr);
                d.setDate(d.getDate() + days); return toDateStr(d); }

            function loadAllData() {
                const raw = localStorage.getItem(DB_KEY);
                if (raw) try { const parsed = JSON.parse(raw);
                    data = { ...data, ...parsed }; } catch (e) {}
                if (!data.items) data.items = [];
                if (!data.completedTasks) data.completedTasks = {};
                if (!data.achievements) data.achievements = [];
                if (!data.dailyCompletions) data.dailyCompletions = {};
                if (!data.fullyMasteredItems) data.fullyMasteredItems = [];
                if (typeof data.points !== 'number') data.points = 0;
                if (typeof data.streak !== 'number') data.streak = 0;
                if (typeof data.totalTasksCompleted !== 'number') data.totalTasksCompleted = 0;
                if (typeof data.penaltyCount !== 'number') data.penaltyCount = 0;
                data.items.forEach(item => { if (!item.reviews) item.reviews = [];
                    ensureReviews(item); });
                const remRaw = localStorage.getItem(REMINDER_KEY);
                if (remRaw) try { reminderSettings = JSON.parse(remRaw); } catch (e) {}
                if (!reminderSettings.times || !Array.isArray(reminderSettings.times)) reminderSettings.times = [
                    '09:00', '20:00'
                ];
                if (typeof reminderSettings.enabled !== 'boolean') reminderSettings.enabled = true;
                lastCheckDate = localStorage.getItem(LAST_CHECK_KEY) || null;
                const trigRaw = localStorage.getItem(LAST_TRIGGER_KEY);
                if (trigRaw) try { lastTriggered = JSON.parse(trigRaw); } catch (e) { lastTriggered = {}; }
            }

            function saveAll() {
                localStorage.setItem(DB_KEY, JSON.stringify(data));
                localStorage.setItem(REMINDER_KEY, JSON.stringify(reminderSettings));
                if (lastCheckDate) localStorage.setItem(LAST_CHECK_KEY, lastCheckDate);
                localStorage.setItem(LAST_TRIGGER_KEY, JSON.stringify(lastTriggered));
            }

            function ensureReviews(item) {
                const existingNums = new Set(item.reviews.map(r => r.num));
                REVIEW_INTERVALS.forEach(ri => { if (!existingNums.has(ri.num)) item.reviews.push({ num: ri.num,
                        date: addDays(item.learnDate, ri.days), completed: false }); });
                item.reviews.sort((a, b) => a.num - b.num);
            }

            function generateId() { return 'it_' + Date.now().toString(36) + '_' + Math.random().toString(36).slice(2,
                6); }

            function getCurrentLevel() {
                let lv = LEVELS[0];
                for (let i = LEVELS.length - 1; i >= 0; i--) { if (data.points >= LEVELS[i].min) { lv = LEVELS[i]; break; } }
                return lv;
            }

            function getTasksForDate(dateStr) {
                const tasks = [];
                data.items.forEach(item => {
                    if (item.learnDate === dateStr) {
                        const ck = dateStr + '::' + item.id + '::0';
                        tasks.push({ type: 'learn', itemId: item.id, topic: item.topic, subject: item.subject,
                            reviewNum: 0, date: dateStr, completed: !!data.completedTasks[ck], label: '📖 初次学习',
                            ck: ck });
                    }
                    item.reviews.forEach(r => {
                        if (r.date === dateStr) {
                            const ck = dateStr + '::' + item.id + '::' + r.num;
                            tasks.push({ type: 'review', itemId: item.id, topic: item.topic, subject: item.subject,
                                reviewNum: r.num, date: dateStr, completed: !!data.completedTasks[ck],
                                label: '🔄 ' + (REVIEW_INTERVALS.find(ri => ri.num === r.num)?.label || ('复习#' + r
                                    .num)), ck: ck });
                        }
                    });
                });
                tasks.sort((a, b) => { if (a.type === 'learn' && b.type !== 'learn') return -1; if (b.type === 'learn' && a
                        .type !== 'learn') return 1; return a.reviewNum - b.reviewNum; });
                return tasks;
            }

            function getTodayTasks() { return getTasksForDate(todayStr); }

            function getOverdueIncompleteCount() {
                const todayD = parseDate(todayStr);
                let count = 0;
                data.items.forEach(item => { item.reviews.forEach(r => { const ck = r.date + '::' + item.id + '::' + r
                        .num; if (!data.completedTasks[ck] && parseDate(r.date) < todayD) count++; }); });
                return count;
            }

            function getCompletionStats() {
                let totalReviews = 0,
                    completedReviews = 0;
                const totalLearnItems = data.items.length;
                data.items.forEach(item => { item.reviews.forEach(r => { totalReviews++; const ck = r.date + '::' + item
                        .id + '::' + r.num; if (data.completedTasks[ck]) completedReviews++; }); });
                const totalTasks = totalLearnItems + totalReviews;
                const overallPct = totalTasks > 0 ? Math.round((completedReviews / totalTasks) * 100) : 0;
                return { totalLearnItems, totalReviews, completedReviews, totalTasks, completedTasks: completedReviews,
                    overallPct };
            }

            function countTasksForDate(dateStr) {
                let count = 0,
                    hasLearn = false,
                    hasReview = false;
                data.items.forEach(item => { if (item.learnDate === dateStr) { count++;
                        hasLearn = true; }
                    item.reviews.forEach(r => { if (r.date === dateStr) { count++;
                            hasReview = true; } }); });
                return { count, hasLearn, hasReview };
            }

            function getTodayIncompleteCount() { return getTodayTasks().filter(t => !t.completed).length; }

            // ==================== 奖励与惩罚核心逻辑 ====================
            function awardPoints(amount, reason) {
                const oldLevel = getCurrentLevel();
                data.points = Math.max(0, data.points + amount);
                saveAll();
                const newLevel = getCurrentLevel();
                if (newLevel.name !== oldLevel.name && amount > 0) {
                    pendingLevelUp = newLevel;
                }
                return { amount, newPoints: data.points, levelUp: pendingLevelUp };
            }

            function penalizePoints(amount, reason) {
                data.penaltyCount = (data.penaltyCount || 0) + 1;
                return awardPoints(-Math.abs(amount), reason);
            }

            function checkAndAwardAchievements() {
                const earned = [];
                const already = new Set(data.achievements.map(a => a.id));

                if (!already.has('first_learn') && data.items.length > 0) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'first_learn'));
                }
                if (!already.has('tasks_10') && data.totalTasksCompleted >= 10) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'tasks_10'));
                }
                if (!already.has('tasks_50') && data.totalTasksCompleted >= 50) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'tasks_50'));
                }
                if (!already.has('tasks_100') && data.totalTasksCompleted >= 100) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'tasks_100'));
                }
                if (!already.has('streak_7') && data.streak >= 7) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'streak_7'));
                }
                if (!already.has('streak_30') && data.streak >= 30) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'streak_30'));
                }
                if (!already.has('full_mastery_1') && data.fullyMasteredItems.length >= 1) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'full_mastery_1'));
                }
                if (!already.has('full_mastery_5') && data.fullyMasteredItems.length >= 5) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'full_mastery_5'));
                }
                if (!already.has('perfect_day') && Object.values(data.dailyCompletions).some(d => d.allDone)) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'perfect_day'));
                }
                if (!already.has('comeback') && data.streakBroken && data.streak >= 3) {
                    earned.push(ACHIEVEMENTS.find(a => a.id === 'comeback'));
                }

                earned.forEach(a => {
                    data.achievements.push({ id: a.id, name: a.name, icon: a.icon, date: todayStr });
                    awardPoints(30, '获得成就: ' + a.name);
                });
                if (earned.length > 0) saveAll();
                return earned;
            }

            function checkFullyMastered(itemId) {
                if (data.fullyMasteredItems.includes(itemId)) return false;
                const item = data.items.find(it => it.id === itemId);
                if (!item) return false;
                const allDone = item.reviews.every(r => {
                    const ck = r.date + '::' + itemId + '::' + r.num;
                    return !!data.completedTasks[ck];
                });
                if (allDone && item.reviews.length >= 7) {
                    data.fullyMasteredItems.push(itemId);
                    awardPoints(100, '完全掌握: ' + item.topic);
                    saveAll();
                    return true;
                }
                return false;
            }

            function processDailyCheckin() {
                if (lastCheckDate === todayStr) return { adjustments: [], rewards: [], penalties: [], newAchievements: [] };
                const yesterdayStr = addDays(todayStr, -1);
                const yesterdayTasks = getTasksForDate(yesterdayStr);
                const todayTasks = getTodayTasks();

                const rewards = [];
                const penalties = [];

                // 检查昨天的完成情况
                if (yesterdayTasks.length > 0) {
                    const yesterdayCompleted = yesterdayTasks.filter(t => t.completed).length;
                    const yesterdayTotal = yesterdayTasks.length;
                    const allDoneYesterday = yesterdayCompleted >= yesterdayTotal;

                    // 记录每日完成情况
                    data.dailyCompletions[yesterdayStr] = {
                        completed: yesterdayCompleted,
                        total: yesterdayTotal,
                        allDone: allDoneYesterday
                    };

                    if (allDoneYesterday) {
                        data.streak = (data.streak || 0) + 1;
                        rewards.push({ type: 'streak', amount: data.streak % 7 === 0 ? 30 : (data.streak % 30 === 0 ? 100 :
                                5), reason: '连续打卡第' + data.streak + '天' });
                        // 连续里程碑
                        if (data.streak === 7) rewards.push({ type: 'milestone', amount: 50, reason: '连续7天！' });
                        if (data.streak === 30) rewards.push({ type: 'milestone', amount: 200, reason: '连续30天！月度冠军！' });
                    } else {
                        if (data.streak > 0) {
                            data.streakBroken = true;
                            penalties.push({ type: 'streak_break', amount: -10, reason: '连续打卡中断 (昨日未完成全部任务)' });
                        }
                        data.streak = 0;
                        if (yesterdayCompleted === 0 && yesterdayTotal > 0) {
                            penalties.push({ type: 'no_activity', amount: -5, reason: '昨日完全未学习' });
                        }
                    }
                } else {
                    // 昨天没有任务，连续天数不受影响
                    if (data.streak > 0 && data.lastActiveDate && parseDate(data.lastActiveDate) < parseDate(addDays(
                            todayStr, -2))) {
                        data.streakBroken = true;
                        penalties.push({ type: 'streak_break', amount: -5, reason: '连续打卡中断 (超过1天无任务)' });
                        data.streak = 0;
                    }
                }

                // 过期任务惩罚
                const overdueCount = getOverdueIncompleteCount();
                if (overdueCount > 0) {
                    const penaltyAmount = Math.min(overdueCount * 3, 50);
                    penalties.push({ type: 'overdue', amount: -penaltyAmount,
                        reason: `${overdueCount}个过期未复习任务` });
                }

                // 应用奖励
                rewards.forEach(r => awardPoints(r.amount, r.reason));
                // 应用惩罚
                penalties.forEach(p => penalizePoints(Math.abs(p.amount), p.reason));

                // 检查成就
                const newAchievements = checkAndAwardAchievements();

                data.lastActiveDate = todayStr;
                lastCheckDate = todayStr;
                saveAll();

                return { adjustments: [], rewards, penalties, newAchievements };
            }

            function autoAdjustOverdueReviews() {
                const todayD = parseDate(todayStr);
                const allAdjustments = [];
                const adjustedItemIds = new Set();
                data.items.forEach(item => {
                    const sortedReviews = [...item.reviews].sort((a, b) => a.num - b.num);
                    let needsRecalc = false;
                    let newBaseDate = null;
                    for (let i = 0; i < sortedReviews.length; i++) {
                        const r = sortedReviews[i];
                        const ck = r.date + '::' + item.id + '::' + r.num;
                        if (!data.completedTasks[ck] && parseDate(r.date) < todayD && !adjustedItemIds.has(
                                item.id + '::' + r.num)) {
                            const oldDate = r.date;
                            r.date = todayStr;
                            needsRecalc = true;
                            if (!newBaseDate) newBaseDate = todayStr;
                            allAdjustments.push({ date: todayStr, itemId: item.id, topic: item.topic, reviewNum: r.num,
                                fromDate: oldDate, toDate: todayStr });
                            adjustedItemIds.add(item.id + '::' + r.num);
                        }
                    }
                    if (needsRecalc && newBaseDate) {
                        const minAdjustedNum = Math.min(...allAdjustments.filter(a => a.itemId === item.id).map(a => a
                            .reviewNum));
                        item.reviews.forEach(r => {
                            if (r.num >= minAdjustedNum) {
                                const ri = REVIEW_INTERVALS.find(ri => ri.num === r.num);
                                if (ri && !adjustedItemIds.has(item.id + '::' + r.num)) {
                                    const newDate = addDays(newBaseDate, ri.days);
                                    if (r.date !== newDate && parseDate(r.date) < todayD) {
                                        allAdjustments.push({ date: todayStr, itemId: item.id, topic: item.topic,
                                            reviewNum: r.num, fromDate: r.date, toDate: newDate });
                                        r.date = newDate;
                                    }
                                }
                            }
                        });
                    }
                });
                if (allAdjustments.length > 0) saveAll();
                return allAdjustments;
            }

            function onTaskComplete(ck, itemId, reviewNum) {
                if (data.completedTasks[ck]) return null; // 已经完成
                data.completedTasks[ck] = true;
                data.totalTasksCompleted = (data.totalTasksCompleted || 0) + 1;
                const result = awardPoints(reviewNum === 0 ? 5 : 10, reviewNum === 0 ? '完成新学习' : '完成复习任务');
                checkFullyMastered(itemId);
                const newAchievements = checkAndAwardAchievements();
                saveAll();
                return { ...result, newAchievements };
            }

            function onTaskUncomplete(ck) {
                if (!data.completedTasks[ck]) return null;
                delete data.completedTasks[ck];
                data.totalTasksCompleted = Math.max(0, (data.totalTasksCompleted || 1) - 1);
                const result = penalizePoints(3, '取消完成任务');
                saveAll();
                return result;
            }

            // ==================== 提醒 ====================
            function updatePageTitle() { const inc = getTodayIncompleteCount();
                document.title = inc > 0 ? `(${inc}) 🧠 艾宾浩斯` : '🧠 艾宾浩斯学习计划'; }

            function updateBottomNavBadge() {
                const inc = getTodayIncompleteCount();
                const todayNav = document.querySelector('.nav-item[data-section="today"]');
                if (!todayNav) return;
                const oldBadge = todayNav.querySelector('.nav-badge');
                if (oldBadge) oldBadge.remove();
                if (inc > 0) { const b = document.createElement('span');
                    b.className = 'nav-badge';
                    b.textContent = inc > 99 ? '99+' : inc;
                    todayNav.style.position = 'relative';
                    todayNav.appendChild(b); }
            }

            function sendNotification(title, body) {
                if ('Notification' in window && Notification.permission === 'granted') {
                    try { const n = new Notification(title, { body, icon: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" width="64" height="64" viewBox="0 0 64 64"%3E%3Ccircle cx="32" cy="32" r="30" fill="%236366f1"/%3E%3Ctext x="32" y="40" text-anchor="middle" font-size="28" fill="white"%3E🧠%3C/text%3E%3C/svg%3E',
                            tag: 'ebbinghaus', requireInteraction: true, vibrate: [200, 100, 200, 100,
                            200] });
                        setTimeout(() => n.close(), 8000); return true; } catch (e) { return false; }
                }
                return false;
            }

            function triggerReminder(timeSlot) {
                const inc = getTodayIncompleteCount();
                if (inc === 0) return;
                const sent = sendNotification('📋 学习提醒', `还有 ${inc} 个任务待完成，请及时复习！`);
                if (!sent) showReminderBanner(inc);
                if ('vibrate' in navigator) try { navigator.vibrate([150, 80, 150, 80, 300]); } catch (e) {}
                lastTriggered[timeSlot] = todayStr;
                localStorage.setItem(LAST_TRIGGER_KEY, JSON.stringify(lastTriggered));
            }

            function showReminderBanner(inc) {
                const ex = document.querySelector('.reminder-banner');
                if (ex) ex.remove();
                const b = document.createElement('div');
                b.className = 'reminder-banner';
                b.innerHTML =
                    `<span>🔔</span><span>您有 <strong>${inc}</strong> 个任务待完成！</span><button class="reminder-close">✕</button>`;
                const app = document.getElementById('app');
                if (app && app.firstChild) app.insertBefore(b, app.firstChild.nextSibling || app.firstChild);
                b.querySelector('.reminder-close').addEventListener('click', e => { e.stopPropagation();
                    b.remove(); });
                b.addEventListener('click', () => { scrollToSection('today');
                    b.remove(); });
                setTimeout(() => { if (b.parentNode) b.remove(); }, 12000);
            }

            function checkReminderTimes() {
                if (!reminderSettings.enabled) return;
                const now = new Date();
                const cm = now.getHours() * 60 + now.getMinutes();
                reminderSettings.times.forEach(ts => { const [h, m] = ts.split(':').map(Number); if (Math.abs(cm - (h *
                        60 + m)) <= 2 && lastTriggered[ts] !== todayStr) triggerReminder(ts); });
            }

            // ==================== 渲染 ====================
            function renderAll(extraFlags = {}) {
                const app = document.getElementById('app');
                const stats = getCompletionStats();
                const todayTasks = getTodayTasks();
                const fourMonthsLater = addDays(todayStr, 120);
                const overdueCount = getOverdueIncompleteCount();
                const currentLevel = getCurrentLevel();
                const levelIdx = LEVELS.indexOf(currentLevel);
                const nextLevel = levelIdx < LEVELS.length - 1 ? LEVELS[levelIdx + 1] : null;
                const pointsToNext = nextLevel ? nextLevel.min - data.points : 0;

                let html = '';
                html += `
            <div class="header">
                <div class="header-top">
                    <div class="header-title">🧠 艾宾浩斯学习计划</div>
                    <div class="header-points"><span class="coin">🪙</span> ${data.points} 分</div>
                </div>
                <div class="header-sub">基于遗忘曲线科学安排复习 · 4个月周期</div>
                <div class="header-badges">
                    <span class="header-badge level-badge">${currentLevel.icon} ${currentLevel.name}</span>
                    ${nextLevel?`<span class="header-badge">📈 距${nextLevel.icon}还需${pointsToNext}分</span>`:''}
                    <span class="header-badge">🔥 连续${data.streak||0}天</span>
                    <span class="header-badge">📅 ${todayStr} → ${fourMonthsLater}</span>
                    ${overdueCount>0?`<span class="header-badge warn">⚠️ ${overdueCount}个过期</span>`:''}
                    ${reminderSettings.enabled?`<span class="header-badge">🔔提醒开</span>`:`<span class="header-badge">🔕提醒关</span>`}
                </div>
            </div>`;

                if (extraFlags.installAvailable && !extraFlags.installDismissed) {
                    html += `
                <div class="install-banner" id="installBanner">
                    📲 <strong>安装到手机</strong> — 像App一样使用，支持离线提醒
                    <button class="install-close" id="dismissInstall">✕</button>
                </div>`;
                }
                if (extraFlags.penalties && extraFlags.penalties.length > 0) {
                    html += `
                <div class="penalty-banner">
                    ⚠️ <strong>惩罚通知：</strong> ${extraFlags.penalties.map(p=>p.reason).join('；')} (共扣${Math.abs(extraFlags.penalties.reduce((s,p)=>s+p.amount,0))}分)
                </div>`;
                }
                if (extraFlags.adjustments && extraFlags.adjustments.length > 0) {
                    html += `
                <div class="reminder-banner" style="background:linear-gradient(135deg,#dbeafe,#bfdbfe);color:#1e40af;border-color:#93c5fd;">
                    🔄 已自动调整 <strong>${extraFlags.adjustments.length}</strong> 个过期复习任务到近期
                </div>`;
                }

                html += `
            <div class="stats-row">
                <div class="stat-card accent"><div class="val" style="color:var(--primary)">${stats.totalLearnItems}</div><div class="lbl">学习条目</div></div>
                <div class="stat-card"><div class="val" style="color:var(--info)">${todayTasks.length}</div><div class="lbl">今日任务</div></div>
                <div class="stat-card"><div class="val" style="color:var(--success)">${stats.overallPct}%</div><div class="lbl">复习完成率</div></div>
                <div class="stat-card gold"><div class="val" style="color:var(--gold)">${data.streak||0}</div><div class="lbl">连续打卡</div></div>
                <div class="stat-card"><div class="val" style="color:var(--primary)">${data.totalTasksCompleted||0}</div><div class="lbl">累计完成</div></div>
            </div>`;

                html += '<div class="main-grid">';
                html += '<div style="display:flex;flex-direction:column;gap:10px;" id="section-items">';
                html += buildAddForm();
                html += buildForgettingCurveCard();
                html += buildAchievementsCard();
                html += buildItemListCard();
                html += '</div>';
                html += '<div style="display:flex;flex-direction:column;gap:10px;" id="section-right">';
                html += '<div id="section-today">' + buildTodayTasksCard(todayTasks) + '</div>';
                html += '<div id="section-calendar">' + buildCalendarCard() + '</div>';
                html += buildReviewScheduleCard();
                html += '<div id="section-settings">' + buildSettingsCard() + '</div>';
                html += '</div>';
                html += '</div>';

                app.innerHTML = html;
                bindEvents();
                updatePageTitle();
                updateBottomNavBadge();
                updateNavActive();

                if (extraFlags.newAchievements && extraFlags.newAchievements.length > 0) {
                    extraFlags.newAchievements.forEach(a => toast('🏆 获得成就: ' + a.icon + ' ' + a.name, 'reward'));
                }
                if (pendingLevelUp) {
                    showLevelUpOverlay(pendingLevelUp);
                    pendingLevelUp = null;
                }
            }

            function buildAddForm() {
                return `
            <div class="card">
                <div class="card-title">➕ 添加学习内容</div>
                <div class="card-subtitle">输入主题，系统自动生成7次艾宾浩斯复习计划</div>
                <div class="form-row"><input type="text" id="topicInput" placeholder="学习主题（如：英语单词Unit3）" maxlength="100" autocomplete="off"></div>
                <div class="form-row">
                    <select id="subjectSelect">${SUBJECTS.map(s=>`<option value="${s}">${s}</option>`).join('')}</select>
                    <input type="date" id="learnDateInput" value="${todayStr}">
                </div>
                <button class="btn primary btn-full" id="btnAddItem">📋 生成学习+复习计划 (+5分奖励)</button>
            </div>`;
            }

            function buildForgettingCurveCard() {
                let barsHTML = '';
                FORGETTING_CURVE.forEach(p => {
                    const isRP = [1, 3, 7, 14, 30, 60, 90].includes(p.day);
                    barsHTML +=
                        `<div class="bar-group"><div class="bar-pct">${p.retention}%</div><div class="bar ${isRP?'reviewed':''}" style="height:${Math.max(p.retention,8)}%;${p.day===0?'background:linear-gradient(180deg,#6366f1,#8b5cf6);':''}"></div><div class="bar-label">${p.label}</div></div>`;
                });
                return `
            <div class="card">
                <div class="card-title">📉 遗忘曲线 & 复习节点</div>
                <div class="curve-chart">${barsHTML}</div>
                <div style="font-size:8px;color:var(--text-muted);">🟣学习 🟢复习节点 🔵遗忘趋势</div>
            </div>`;
            }

            function buildAchievementsCard() {
                const earnedIds = new Set(data.achievements.map(a => a.id));
                const chips = ACHIEVEMENTS.map(a => {
                    const isEarned = earnedIds.has(a.id);
                    const earnedData = data.achievements.find(e => e.id === a.id);
                    return `<span class="achievement-chip ${isEarned?'earned':''}" title="${a.desc}${isEarned&&earnedData?' · 获得于'+earnedData.date:''}">${a.icon} ${a.name}${isEarned?' ✅':''}</span>`;
                }).join('');
                return `
            <div class="card">
                <div class="card-title">🏆 成就徽章 (${data.achievements.length}/${ACHIEVEMENTS.length})</div>
                <div class="achievement-grid">${chips}</div>
                <div style="font-size:8px;color:var(--text-muted);margin-top:4px;">完成里程碑解锁成就 · 每个成就+30分</div>
            </div>`;
            }

            function buildItemListCard() {
                const items = [...data.items].sort((a, b) => (b.createdAt || '').localeCompare(a.createdAt || ''));
                let rows = '';
                if (items.length === 0) rows =
                    '<div style="text-align:center;padding:20px;color:var(--text-muted);font-size:11px;">📝 暂无学习条目</div>';
                else items.forEach(item => {
                    const tr = item.reviews.length;
                    const cr = item.reviews.filter(r => { const ck = r.date + '::' + item.id + '::' + r.num; return !!data
                            .completedTasks[ck]; }).length;
                    const pct = tr > 0 ? Math.round((cr / tr) * 100) : 0;
                    const isDone = pct === 100;
                    rows += `
                <div class="item-row">
                    <div class="item-dot ${isDone?'review':'learn'}"></div>
                    <div class="item-info"><div class="item-name">${esc(item.topic)}</div><div class="item-meta">${item.subject} · ${item.learnDate} · ${cr}/${tr}</div></div>
                    <span class="item-progress ${isDone?'done':''}">${pct}%</span>
                    <button class="btn xs danger" data-action="delItem" data-id="${item.id}">🗑️</button>
                </div>`;
                });
                return `
            <div class="card">
                <div class="card-title">📚 学习条目 (${data.items.length})</div>
                <div class="item-list">${rows}</div>
                ${data.items.length>0?`<button class="btn secondary btn-full" id="btnClearAll" style="margin-top:6px;font-size:9px;">🗑️ 清空全部</button>`:''}
            </div>`;
            }

            function buildCalendarCard() {
                if (!calYear) { calYear = today.getFullYear();
                    calMonth = today.getMonth(); }
                const mn = ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'];
                const fd = new Date(calYear, calMonth, 1);
                const ld = new Date(calYear, calMonth + 1, 0);
                const sdow = fd.getDay();
                const td = ld.getDate();
                let cells = '';
                const pld = new Date(calYear, calMonth, 0);
                for (let i = sdow - 1; i >= 0; i--) cells +=
                    `<div class="cal-cell other-month"><span>${pld.getDate()-i}</span></div>`;
                for (let d = 1; d <= td; d++) {
                    const ds = toDateStr(new Date(calYear, calMonth, d));
                    const info = countTasksForDate(ds);
                    const isT = ds === todayStr;
                    const hasT = info.count > 0;
                    let dots = '';
                    if (info.hasLearn) dots += '<span class="task-dot learn"></span>';
                    if (info.hasReview) dots += '<span class="task-dot review"></span>';
                    cells +=
                        `<div class="cal-cell ${isT?'today':''} ${hasT?'has-tasks':''}" data-date="${ds}">${info.count>0?`<span class="task-count">${info.count}</span>`:''}<span>${d}</span>${dots?`<div class="task-dots">${dots}</div>`:''}</div>`;
                }
                const rem = (sdow + td) <= 35 ? 35 - (sdow + td) : 42 - (sdow + td);
                for (let d = 1; d <= rem; d++) cells += `<div class="cal-cell other-month"><span>${d}</span></div>`;
                return `
            <div class="card">
                <div class="cal-nav">
                    <button id="calPrev">◀</button><span class="month-label">${calYear}年 ${mn[calMonth]}</span><button id="calNext">▶</button>
                    <button class="btn secondary sm" id="calToday">📌今天</button>
                </div>
                <div class="cal-grid">
                    <div class="cal-day-header">日</div><div class="cal-day-header">一</div><div class="cal-day-header">二</div><div class="cal-day-header">三</div><div class="cal-day-header">四</div><div class="cal-day-header">五</div><div class="cal-day-header">六</div>
                    ${cells}
                </div>
            </div>`;
            }

            function buildTodayTasksCard(tasks) {
                let rows = '';
                if (tasks.length === 0) rows =
                    '<div style="text-align:center;padding:18px;color:var(--text-muted);font-size:11px;">🎉 今日无任务</div>';
                else tasks.forEach(t => {
                    rows += `
                <div class="today-task-item ${t.completed?'completed':''}" data-ck="${t.ck}" data-type="${t.type}" data-itemid="${t.itemId}" data-reviewnum="${t.reviewNum}">
                    <div class="check-circle">✓</div>
                    <span style="flex:1;min-width:0;overflow:hidden;text-overflow:ellipsis;">${esc(t.topic)}</span>
                    <span class="task-type-badge ${t.type==='learn'?'new':'review'}">${t.type==='learn'?'📖新学':'🔄复习'}</span>
                    ${t.type==='review'?`<span style="font-size:8px;color:var(--text-muted);">#${t.reviewNum}</span>`:''}
                </div>`;
                });
                const inc = tasks.filter(t => !t.completed).length;
                return `
            <div class="card">
                <div class="card-title">📋 今日任务 ${inc>0?`<span style="color:#ef4444;font-size:9px;">· ${inc}待完成</span>`:'<span style="color:#10b981;font-size:9px;">· ✅全部完成</span>'}</div>
                <div class="today-task-list">${rows}</div>
                <div style="font-size:8px;color:var(--text-muted);margin-top:4px;">💡 点击标记完成 (+10分复习/+5分新学) | 取消扣3分</div>
            </div>`;
            }

            function buildReviewScheduleCard() {
                return `
            <div class="card">
                <div class="card-title">⏱ 复习间隔 & 奖励规则</div>
                <div style="font-size:9px;color:var(--text-muted);margin-bottom:4px;">每次复习+10分 | 新学+5分 | 完全掌握+100分</div>
                ${REVIEW_INTERVALS.map(ri=>`<div style="display:flex;align-items:center;gap:6px;padding:3px 0;font-size:10px;border-bottom:1px solid var(--border);"><span style="font-weight:700;color:var(--primary);">#${ri.num}</span><span style="flex:1;">${ri.label}</span><span style="color:var(--text-muted);font-size:8px;">+${ri.days}天</span></div>`).join('')}
                <div style="font-size:8px;color:#ef4444;margin-top:4px;">⚠️ 过期未复习：每项-3分 | 连续中断：-10分</div>
            </div>`;
            }

            function buildSettingsCard() {
                const timeHTML = reminderSettings.times.map((t, i) => `
                <div class="form-row" style="margin-bottom:3px;">
                    <input type="time" class="reminder-time-input" value="${t}" data-index="${i}" style="flex:1;">
                    <button class="btn xs danger remove-time-btn" data-index="${i}" ${reminderSettings.times.length<=1?'disabled':''}>✕</button>
                </div>`).join('');
                const notifStatus = 'Notification' in window ? (Notification.permission === 'granted' ? '✅ 已授权' :
                    Notification.permission === 'denied' ? '❌ 已拒绝' : '⏳ 待授权') : '⚠️ 不支持';
                return `
            <div class="card">
                <div class="card-title">⚙️ 设置</div>
                <div class="settings-section">
                    <div class="setting-row"><div><div class="setting-label">🔔 每日提醒</div><div class="setting-sublabel">到时间提醒未完成任务</div></div><label class="toggle-switch"><input type="checkbox" id="reminderToggle" ${reminderSettings.enabled?'checked':''}><span class="toggle-slider"></span></label></div>
                    <div class="setting-row" style="${reminderSettings.enabled?'':'opacity:0.4;pointer-events:none;'}"><div style="width:100%;"><div class="setting-label">⏰ 提醒时间</div>${timeHTML}<button class="btn secondary xs" id="addReminderTime" style="margin-top:3px;">+ 添加</button></div></div>
                    <div class="setting-row"><div><div class="setting-label">🔧 手动调整</div><div class="setting-sublabel">调整过期未完成的复习</div></div><button class="btn warn sm" id="btnManualAdjust">立即调整</button></div>
                    <div class="setting-row"><div><div class="setting-label">🔔 通知权限</div><div class="setting-sublabel">${notifStatus}</div></div><button class="btn secondary sm" id="btnRequestNotif">请求权限</button></div>
                    <div class="setting-row"><div><div class="setting-label">🪙 积分管理</div><div class="setting-sublabel">当前: ${data.points}分 | ${getCurrentLevel().icon} ${getCurrentLevel().name}</div></div><button class="btn secondary sm" id="btnResetPoints">重置积分</button></div>
                </div>
            </div>`;
            }

            function esc(s) { const d = document.createElement('div');
                d.textContent = s || ''; return d.innerHTML; }

            function showLevelUpOverlay(level) {
                const ov = document.createElement('div');
                ov.className = 'level-up-overlay';
                ov.innerHTML = `
                <div class="level-up-card">
                    <div class="big-emoji">${level.icon}</div>
                    <h2 style="margin:8px 0;font-size:20px;">🎉 升级啦！</h2>
                    <p style="font-size:14px;font-weight:700;">${level.name}</p>
                    <p style="font-size:11px;color:#92400e;">积分: ${data.points} 分</p>
                    <button class="btn primary" style="margin-top:10px;" id="closeLevelUp">太棒了！</button>
                </div>`;
                document.body.appendChild(ov);
                ov.querySelector('#closeLevelUp').addEventListener('click', () => ov.remove());
                ov.addEventListener('click', e => { if (e.target === ov) ov.remove(); });
                setTimeout(() => { if (ov.parentNode) ov.remove(); }, 5000);
                if ('vibrate' in navigator) try { navigator.vibrate([100, 50, 100, 50, 200]); } catch (e) {}
            }

            // ==================== 事件 ====================
            function bindEvents() {
                document.getElementById('btnAddItem')?.addEventListener('click', addItem);
                document.getElementById('topicInput')?.addEventListener('keydown', function(e) { if (e.key === 'Enter') { e
                        .preventDefault();
                    addItem(); } });
                document.querySelectorAll('[data-action="delItem"]').forEach(b => b.addEventListener('click', function() {
                    deleteItem(this.dataset.id); }));
                document.getElementById('btnClearAll')?.addEventListener('click', () => { if (!confirm(
                        '确定清空所有学习条目？此操作不可恢复。')) return;
                    data.items = [];
                    data.completedTasks = {};
                    data.fullyMasteredItems = [];
                    saveAll();
                    renderAll();
                    toast('🗑️ 已清空'); });
                document.getElementById('calPrev')?.addEventListener('click', () => { calMonth--; if (calMonth < 0) { calMonth =
                        11;
                        calYear--; }
                    renderAll(); });
                document.getElementById('calNext')?.addEventListener('click', () => { calMonth++; if (calMonth > 11) { calMonth =
                        0;
                        calYear++; }
                    renderAll(); });
                document.getElementById('calToday')?.addEventListener('click', () => { calYear = today.getFullYear();
                    calMonth = today.getMonth();
                    renderAll();
                    scrollToSection('calendar'); });
                document.querySelectorAll('.cal-cell:not(.other-month)').forEach(c => c.addEventListener('click',
                function() { const ds = this.dataset.date; if (ds) showDateDetailModal(ds); }));
                document.querySelectorAll('.today-task-item').forEach(item => item.addEventListener('click', function() {
                    const ck = this.dataset.ck;
                    const itemId = this.dataset.itemid;
                    const reviewNum = parseInt(this.dataset.reviewnum);
                    if (!ck) return;
                    if (data.completedTasks[ck]) {
                        const res = onTaskUncomplete(ck);
                        if (res) toast(`⚠️ -${Math.abs(res.amount)}分 · ${res.levelUp?'升级!':''}`,
                            'penalty');
                    } else {
                        const res = onTaskComplete(ck, itemId, reviewNum);
                        if (res) {
                            toast(`✅ +${res.amount}分！`, 'reward');
                            if (res.newAchievements && res.newAchievements.length > 0) {
                                res.newAchievements.forEach(a => toast('🏆 ' + a.icon + ' ' + a.name,
                                    'reward'));
                            }
                        }
                    }
                    saveAll();
                    renderAll({ newAchievements: [] });
                    updatePageTitle();
                    updateBottomNavBadge();
                    if (pendingLevelUp) { showLevelUpOverlay(pendingLevelUp);
                        pendingLevelUp = null; }
                }));
                document.getElementById('reminderToggle')?.addEventListener('change', function() { reminderSettings
                        .enabled = this.checked;
                    saveAll();
                    renderAll();
                    toast(this.checked ? '🔔 提醒已开启' : '🔕 提醒已关闭'); if (this.checked && 'Notification' in window &&
                        Notification.permission === 'default') Notification.requestPermission(); });
                document.querySelectorAll('.reminder-time-input').forEach(inp => inp.addEventListener('change',
                function() { const i = parseInt(this.dataset.index); if (i >= 0 && i < reminderSettings.times
                        .length) { reminderSettings.times[i] = this.value;
                        saveAll();
                        toast('⏰ 已更新'); } }));
                document.querySelectorAll('.remove-time-btn').forEach(b => b.addEventListener('click', function() { const i =
                    parseInt(this.dataset.index); if (i >= 0 && reminderSettings.times.length > 1) { reminderSettings
                        .times.splice(i, 1);
                    saveAll();
                    renderAll();
                    toast('已移除'); } }));
                document.getElementById('addReminderTime')?.addEventListener('click', () => { if (reminderSettings.times
                        .length >= 5) { toast('最多5个'); return; }
                    reminderSettings.times.push('12:00');
                    saveAll();
                    renderAll();
                    toast('已添加'); });
                document.getElementById('btnManualAdjust')?.addEventListener('click', () => { const adj =
                    autoAdjustOverdueReviews(); if (adj.length > 0) { lastCheckDate = todayStr;
                        saveAll();
                        renderAll({ adjustments: adj });
                        toast(`🔄 已调整${adj.length}个任务`); } else toast('✅ 无需调整'); });
                document.getElementById('btnRequestNotif')?.addEventListener('click', () => { if ('Notification' in
                    window) Notification.requestPermission().then(p => { renderAll();
                        toast(p === 'granted' ? '✅ 已开启' : '⚠️ 已拒绝'); }); });
                document.getElementById('btnResetPoints')?.addEventListener('click', () => { if (confirm(
                        '确定重置积分？等级和成就将保留。')) { data.points = 0;
                        saveAll();
                        renderAll();
                        toast('🪙 积分已重置'); } });
                document.querySelectorAll('.nav-item').forEach(ni => ni.addEventListener('click', function() { const s =
                    this.dataset.section;
                    currentSection = s;
                    scrollToSection(s);
                    updateNavActive(); }));
                document.getElementById('installBanner')?.addEventListener('click', async () => { if (
                        deferredInstallPrompt) { deferredInstallPrompt.prompt(); const { outcome } = await deferredInstallPrompt
                            .userChoice;
                        deferredInstallPrompt = null;
                        toast(outcome === 'accepted' ? '✅ 正在安装...' : '已取消'); }
                    document.getElementById('installBanner')?.remove(); });
                document.getElementById('dismissInstall')?.addEventListener('click', e => { e.stopPropagation();
                    document.getElementById('installBanner')?.remove();
                    localStorage.setItem('ebbinghaus_install_dismissed', todayStr); });
                document.addEventListener('click', function(e) { if (e.target.classList.contains('reminder-close')) e
                        .target.closest('.reminder-banner')?.remove(); });
            }

            function scrollToSection(s) {
                const el = document.getElementById(s === 'today' ? 'section-today' : s === 'calendar' ? 'section-calendar' :
                    s === 'items' ? 'section-items' : 'section-settings');
                if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' });
                currentSection = s;
                updateNavActive();
            }

            function updateNavActive() { document.querySelectorAll('.nav-item').forEach(ni => ni.classList.toggle('active',
                    ni.dataset.section === currentSection)); }

            function addItem() {
                const topic = document.getElementById('topicInput')?.value.trim();
                if (!topic) { toast('⚠️ 请输入主题'); return; }
                const subject = document.getElementById('subjectSelect')?.value || '📝 自定义';
                const ld = document.getElementById('learnDateInput')?.value || todayStr;
                const item = { id: generateId(), topic, subject, learnDate: ld, reviews: [], createdAt: new Date()
                        .toISOString() };
                ensureReviews(item);
                data.items.push(item);
                awardPoints(5, '添加学习条目');
                const newAch = checkAndAwardAchievements();
                saveAll();
                document.getElementById('topicInput').value = '';
                renderAll({ newAchievements: newAch });
                updatePageTitle();
                updateBottomNavBadge();
                toast('✅ 计划已生成！+5分');
                if (pendingLevelUp) { showLevelUpOverlay(pendingLevelUp);
                    pendingLevelUp = null; }
            }

            function deleteItem(id) {
                const item = data.items.find(it => it.id === id);
                if (!item || !confirm(`删除「${item.topic}」？`)) return;
                Object.keys(data.completedTasks).forEach(ck => { if (ck.includes('::' + id + '::')) delete data
                        .completedTasks[ck]; });
                data.items = data.items.filter(it => it.id !== id);
                data.fullyMasteredItems = data.fullyMasteredItems.filter(fid => fid !== id);
                saveAll();
                renderAll();
                toast('🗑️ 已删除');
            }

            function showDateDetailModal(dateStr) {
                const tasks = getTasksForDate(dateStr);
                let rows = tasks.length === 0 ?
                    '<div style="text-align:center;padding:14px;color:var(--text-muted);">无任务</div>' :
                    tasks.map(t => `
                <div class="day-task-row"><span style="flex:1;">${t.type==='learn'?'📖':'🔄'} ${esc(t.topic)} ${t.type==='review'?'·复习#'+t.reviewNum:''}</span><span class="task-type-badge ${t.type==='learn'?'new':'review'}">${t.type==='learn'?'新学':'复习'}</span><span style="font-size:9px;color:${t.completed?'var(--success)':'var(--text-muted)'};">${t.completed?'✅':'⏳'}</span></div>
                `).join('');
                const ov = document.createElement('div');
                ov.className = 'modal-overlay';
                ov.innerHTML =
                    `<div class="modal" style="position:relative;"><button class="modal-close-btn" id="closeModal">✕</button><h4>📅 ${dateStr}</h4><div style="max-height:50vh;overflow-y:auto;">${rows}</div><div style="text-align:right;margin-top:8px;"><button class="btn secondary sm" id="closeModalBtn">关闭</button></div></div>`;
                document.body.appendChild(ov);
                const close = () => ov.remove();
                ov.addEventListener('click', e => { if (e.target === ov) close(); });
                ov.querySelector('#closeModal')?.addEventListener('click', close);
                ov.querySelector('#closeModalBtn')?.addEventListener('click', close);
                document.addEventListener('keydown', function eh(e) { if (e.key === 'Escape') { close();
                        document.removeEventListener('keydown', eh); } });
            }

            function toast(msg, type = '') {
                const wrap = document.getElementById('toastWrap');
                const t = document.createElement('div');
                t.className = 'toast ' + (type || '');
                t.textContent = msg;
                wrap.appendChild(t);
                setTimeout(() => t.remove(), 2200);
            }

            // ==================== PWA ====================
            function setupPWA() {
                // 动态创建manifest
                const manifest = {
                    name: '艾宾浩斯学习计划',
                    short_name: '艾宾浩斯',
                    description: '基于遗忘曲线的科学学习工具',
                    start_url: location.href.split('?')[0],
                    display: 'standalone',
                    orientation: 'portrait',
                    background_color: '#e8ecf1',
                    theme_color: '#6366f1',
                    icons: [
                        { src: 'data:image/svg+xml,' + encodeURIComponent(
                                '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 192 192"><circle cx="96" cy="96" r="92" fill="#6366f1"/><text x="96" y="118" text-anchor="middle" font-size="72">🧠</text></svg>'
                                ), sizes: '192x192', type: 'image/svg+xml', purpose: 'any maskable' },
                        { src: 'data:image/svg+xml,' + encodeURIComponent(
                                '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><circle cx="256" cy="256" r="248" fill="#6366f1"/><text x="256" y="310" text-anchor="middle" font-size="180">🧠</text></svg>'
                                ), sizes: '512x512', type: 'image/svg+xml', purpose: 'any maskable' }
                    ]
                };
                const blob = new Blob([JSON.stringify(manifest)], { type: 'application/json' });
                const manifestURL = URL.createObjectURL(blob);
                const link = document.createElement('link');
                link.rel = 'manifest';
                link.href = manifestURL;
                document.head.appendChild(link);

                // 监听安装事件
                window.addEventListener('beforeinstallprompt', (e) => {
                    e.preventDefault();
                    deferredInstallPrompt = e;
                    const dismissed = localStorage.getItem('ebbinghaus_install_dismissed');
                    if (dismissed !== todayStr) {
                        renderAll({ installAvailable: true, installDismissed: false });
                    }
                });
                window.addEventListener('appinstalled', () => {
                    deferredInstallPrompt = null;
                    toast('✅ 应用已安装到主屏幕！');
                });
            }

            // ==================== 初始化 ====================
            function init() {
                loadAllData();
                calYear = today.getFullYear();
                calMonth = today.getMonth();
                data.items.forEach(item => ensureReviews(item));
                saveAll();

                // 每日签到检查
                const checkinResult = processDailyCheckin();
                const adjustments = autoAdjustOverdueReviews();
                if (adjustments.length > 0) saveAll();

                const allPenalties = [...(checkinResult.penalties || [])];
                const allRewards = [...(checkinResult.rewards || [])];
                const allAchievements = [...(checkinResult.newAchievements || [])];

                // 展示奖励
                if (allRewards.length > 0) {
                    const totalReward = allRewards.reduce((s, r) => s + r.amount, 0);
                    setTimeout(() => toast(`🎁 签到奖励: +${totalReward}分！连续${data.streak}天`, 'reward'), 500);
                }

                renderAll({
                    adjustments: adjustments.length > 0 ? adjustments : null,
                    penalties: allPenalties.length > 0 ? allPenalties : null,
                    newAchievements: allAchievements,
                    installAvailable: !!deferredInstallPrompt,
                    installDismissed: localStorage.getItem('ebbinghaus_install_dismissed') === todayStr
                });

                updatePageTitle();
                updateBottomNavBadge();

                // 设置提醒检查
                setInterval(checkReminderTimes, 30000);
                setTimeout(checkReminderTimes, 2000);

                document.addEventListener('visibilitychange', () => {
                    if (document.visibilityState === 'visible') {
                        updatePageTitle();
                        updateBottomNavBadge();
                        const newAdj = autoAdjustOverdueReviews();
                        if (newAdj.length > 0) { saveAll();
                            renderAll({ adjustments: newAdj }); }
                        checkReminderTimes();
                    }
                });

                setupPWA();

                // 请求通知权限
                if ('Notification' in window && Notification.permission === 'default') {
                    setTimeout(() => Notification.requestPermission(), 4000);
                }

                console.log('🧠 艾宾浩斯学习计划 v3.0 已就绪');
                console.log('🪙 积分:', data.points, '|', getCurrentLevel().name);
                console.log('🔥 连续:', data.streak, '天 | 🏆 成就:', data.achievements.length);
                console.log('📲 PWA:', deferredInstallPrompt ? '可安装' : '已安装或不可用');
                console.log('🎁 奖励规则: 复习+10分 | 新学+5分 | 完全掌握+100分 | 成就+30分');
                console.log('⚠️ 惩罚规则: 过期-3分/项 | 中断-10分 | 无活动-5分');
            }

            init();
        })();
    </script>
</body>
</html>
