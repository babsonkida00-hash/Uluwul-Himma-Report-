<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Student Report Sheet Generator</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 10px;
            -webkit-font-smoothing: antialiased;
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
        }

        /* Auth Styles */
        .auth-container {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            max-width: 100%;
            margin: 20px auto;
            animation: slideUp 0.5s ease;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .auth-header {
            text-align: center;
            margin-bottom: 25px;
        }

        .auth-header h1 {
            color: #333;
            font-size: 24px;
            margin-bottom: 8px;
        }

        .auth-header p {
            color: #666;
            font-size: 14px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 6px;
            color: #333;
            font-weight: 600;
            font-size: 13px;
        }

        .form-group input {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            transition: all 0.3s;
            -webkit-appearance: none;
        }

        .form-group input:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .btn {
            width: 100%;
            padding: 14px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            margin-bottom: 10px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            -webkit-appearance: none;
            touch-action: manipulation;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-secondary {
            background: #f0f0f0;
            color: #333;
        }

        .btn-success {
            background: #28a745;
            color: white;
        }

        .btn-danger {
            background: #dc3545;
            color: white;
        }

        .btn-info {
            background: #17a2b8;
            color: white;
        }

        .btn-warning {
            background: #ffc107;
            color: #212529;
        }

        .btn-sm {
            width: auto;
            padding: 8px 12px;
            font-size: 13px;
        }

        .auth-links {
            text-align: center;
            margin-top: 15px;
            font-size: 13px;
        }

        .auth-links a {
            color: #667eea;
            text-decoration: none;
            cursor: pointer;
            font-weight: 600;
        }

        .hidden {
            display: none !important;
        }

        /* Developer Button */
        .developer-btn {
            position: fixed;
            bottom: 15px;
            right: 15px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 10px 15px;
            border-radius: 25px;
            border: none;
            cursor: pointer;
            font-size: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            z-index: 999;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        /* Developer Modal */
        .developer-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.85);
            z-index: 1001;
            display: none;
            align-items: center;
            justify-content: center;
            padding: 15px;
        }

        .developer-modal.active {
            display: flex;
        }

        .developer-content {
            background: white;
            border-radius: 15px;
            padding: 25px;
            max-width: 100%;
            width: 100%;
            max-height: 85vh;
            overflow-y: auto;
            position: relative;
        }

        .developer-header {
            text-align: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid #667eea;
        }

        .developer-avatar {
            width: 70px;
            height: 70px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 50%;
            margin: 0 auto 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 36px;
        }

        .contact-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 10px;
            margin-top: 15px;
        }

        .contact-item {
            background: #f8f9fa;
            padding: 12px;
            border-radius: 8px;
            text-align: center;
            font-size: 13px;
        }

        .contact-item strong {
            display: block;
            color: #667eea;
            margin-bottom: 3px;
            font-size: 12px;
        }

        .close-developer {
            position: absolute;
            top: 10px;
            right: 10px;
            background: #dc3545;
            color: white;
            border: none;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 16px;
        }

        /* Main App Styles */
        .app-container {
            display: none;
        }

        .app-header {
            background: white;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .app-header h2 {
            color: #333;
            font-size: 16px;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 13px;
        }

        .section {
            background: white;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateX(20px); }
            to { opacity: 1; transform: translateX(0); }
        }

        .section-header {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 12px;
            border-bottom: 2px solid #667eea;
        }

        .section-number {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-right: 12px;
            font-size: 14px;
        }

        .section h3 {
            color: #333;
            font-size: 18px;
        }

        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .input-group {
            display: flex;
            gap: 8px;
            align-items: flex-end;
        }

        .subject-list {
            margin-top: 15px;
        }

        .subject-item {
            background: #f8f9fa;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 14px;
        }

        .subject-info {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .subject-name {
            font-weight: 600;
            color: #333;
        }

        .subject-max {
            background: #667eea;
            color: white;
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 11px;
        }

        .btn-remove {
            background: #dc3545;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 11px;
        }

        .empty-state {
            text-align: center;
            padding: 30px;
            color: #999;
            font-style: italic;
            font-size: 14px;
        }

        .students-container {
            margin-top: 15px;
        }

        .student-card {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 18px;
            margin-bottom: 15px;
            border: 1px solid #e0e0e0;
        }

        .student-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 12px;
            border-bottom: 1px solid #e0e0e0;
        }

        .student-name-input {
            font-size: 16px;
            font-weight: 600;
            border: none;
            background: transparent;
            border-bottom: 2px solid #667eea;
            padding: 5px;
            width: 100%;
            max-width: 200px;
        }

        .scores-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }

        .score-input-group {
            background: white;
            padding: 12px;
            border-radius: 8px;
            border: 1px solid #e0e0e0;
        }

        .score-input-group label {
            display: block;
            font-size: 11px;
            color: #666;
            margin-bottom: 4px;
            font-weight: 600;
        }

        .score-input-wrapper {
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .score-input {
            width: 60px;
            padding: 6px;
            border: 2px solid #e0e0e0;
            border-radius: 5px;
            text-align: center;
            font-weight: 600;
            font-size: 14px;
        }

        .score-input:focus {
            outline: none;
            border-color: #667eea;
        }

        .score-input.error {
            border-color: #dc3545;
            background: #fff5f5;
        }

        .max-label {
            color: #999;
            font-size: 11px;
        }

        .navigation {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            padding-top: 15px;
            border-top: 1px solid #e0e0e0;
        }

        .btn-nav {
            width: auto;
            padding: 10px 20px;
            font-size: 14px;
        }

        .btn-next {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-prev {
            background: #6c757d;
            color: white;
        }

        .progress-bar {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
            gap: 8px;
        }

        .progress-step {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            background: #e0e0e0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #999;
            position: relative;
            font-size: 14px;
        }

        .progress-step.active {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .progress-step.completed {
            background: #28a745;
            color: white;
        }

        .progress-step::after {
            content: '';
            position: absolute;
            width: 40px;
            height: 2px;
            background: #e0e0e0;
            left: 32px;
            top: 50%;
            transform: translateY(-50%);
        }

        .progress-step:last-child::after {
            display: none;
        }

        .generate-section {
            text-align: center;
            padding: 25px;
        }

        .btn-generate {
            background: linear-gradient(135deg, #28a745 0%, #20c997 100%);
            color: white;
            padding: 16px 35px;
            font-size: 16px;
            border-radius: 30px;
            border: none;
            cursor: pointer;
            box-shadow: 0 6px 20px rgba(40, 167, 69, 0.3);
        }

        .action-buttons {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-top: 15px;
        }

        .btn-download-all, .btn-print-all {
            padding: 14px 20px;
            font-size: 15px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-weight: 600;
        }

        .btn-download-all {
            background: #dc3545;
            color: white;
        }

        .btn-print-all {
            background: #17a2b8;
            color: white;
        }

        .btn-save-list {
            background: #ffc107;
            color: #212529;
            padding: 10px 16px;
            font-size: 13px;
            border-radius: 6px;
            border: none;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            margin-bottom: 10px;
        }

        .saved-lists {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 15px;
        }

        .saved-list-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px;
            background: white;
            border-radius: 6px;
            margin-bottom: 8px;
            border: 1px solid #e0e0e0;
            font-size: 13px;
        }

        /* Logo Upload Styles */
        .logo-upload {
            border: 2px dashed #667eea;
            border-radius: 8px;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            background: #f8f9fa;
        }

        .logo-preview {
            max-width: 80px;
            max-height: 80px;
            margin: 8px auto;
            display: none;
            border-radius: 8px;
        }

        .logo-preview.active {
            display: block;
        }

        .remove-logo {
            color: #dc3545;
            font-size: 11px;
            cursor: pointer;
            margin-top: 5px;
            display: none;
        }

        .remove-logo.active {
            display: inline-block;
        }

        /* Report Sheet Styles */
        .report-sheet {
            width: 100%;
            max-width: 210mm;
            background: white;
            margin: 0 auto 30px;
            padding: 15px;
            box-shadow: 0 0 15px rgba(0,0,0,0.2);
            position: relative;
            page-break-after: always;
        }

        .watermark {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) rotate(-45deg);
            font-size: 48px;
            color: rgba(0, 102, 204, 0.06);
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 8px;
            pointer-events: none;
            z-index: 0;
            white-space: nowrap;
            user-select: none;
        }

        .report-content {
            position: relative;
            z-index: 1;
        }

        .report-header-with-logos {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 15px;
            border-bottom: 2px solid #0066cc;
            padding-bottom: 15px;
        }

        .report-logo {
            width: 50px;
            height: 50px;
            object-fit: contain;
            border-radius: 6px;
        }

        .report-logo-placeholder {
            width: 50px;
            height: 50px;
            background: #0066cc;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 24px;
        }

        .report-header-center {
            flex: 1;
            text-align: center;
            padding: 0 10px;
        }

        .school-name {
            font-size: 16px;
            font-weight: bold;
            color: #333;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 3px;
        }

        .school-address {
            color: #666;
            font-size: 11px;
            margin-bottom: 6px;
        }

        .report-title {
            background: #0066cc;
            color: white;
            padding: 5px 12px;
            font-weight: bold;
            letter-spacing: 2px;
            display: inline-block;
            border-radius: 4px;
            font-size: 12px;
        }

        .student-info-grid {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr;
            gap: 10px;
            margin-bottom: 20px;
            background: #f8f9fa;
            padding: 12px;
            border-radius: 8px;
        }

        .info-item {
            display: flex;
            flex-direction: column;
        }

        .info-label {
            font-size: 10px;
            color: #666;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 3px;
        }

        .info-value {
            font-size: 13px;
            font-weight: bold;
            color: #333;
        }

        .results-table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
            font-size: 12px;
        }

        .results-table th {
            background: #0066cc;
            color: white;
            padding: 8px;
            text-align: left;
            font-weight: 600;
        }

        .results-table td {
            padding: 8px;
            border-bottom: 1px solid #e0e0e0;
        }

        .results-table tr:nth-child(even) {
            background: #f8f9fa;
        }

        .grade {
            font-weight: bold;
            padding: 2px 8px;
            border-radius: 3px;
            display: inline-block;
            font-size: 11px;
        }

        .grade-a { background: #d4edda; color: #155724; }
        .grade-b { background: #cce5ff; color: #004085; }
        .grade-c { background: #fff3cd; color: #856404; }
        .grade-d { background: #f8d7da; color: #721c24; }
        .grade-e { background: #f5c6cb; color: #721c24; }
        .grade-f { background: #f5c6cb; color: #721c24; }

        .summary-box {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 20px 0;
        }

        .summary-item {
            background: #f8f9fa;
            padding: 12px;
            border-radius: 8px;
            text-align: center;
            border: 1px solid #e0e0e0;
        }

        .summary-label {
            font-size: 10px;
            color: #666;
            text-transform: uppercase;
            margin-bottom: 6px;
        }

        .summary-value {
            font-size: 20px;
            font-weight: bold;
            color: #0066cc;
        }

        .summary-grade {
            font-size: 32px;
            color: #28a745;
        }

        .remarks {
            background: #fff3cd;
            padding: 12px;
            border-radius: 8px;
            margin: 15px 0;
            text-align: center;
            font-weight: 600;
            color: #856404;
            font-size: 13px;
        }

        .signatures {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid #e0e0e0;
        }

        .signature-line {
            text-align: center;
        }

        .signature-line .line {
            border-top: 1px solid #333;
            margin-bottom: 6px;
            padding-top: 6px;
            font-size: 11px;
        }

        .signature-line .title {
            font-size: 9px;
            color: #666;
        }

        .term-info {
            background: #fff3cd;
            padding: 10px;
            border-radius: 6px;
            margin-top: 20px;
            font-size: 11px;
        }

        /* Print Preview Modal */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 1000;
            display: none;
            overflow-y: auto;
            padding: 10px;
        }

        .modal-overlay.active {
            display: block;
        }

        .modal-content {
            background: white;
            max-width: 100%;
            margin: 0 auto;
            border-radius: 8px;
            padding: 15px;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 15px;
            border-bottom: 1px solid #e0e0e0;
            position: sticky;
            top: 0;
            background: white;
            z-index: 10;
        }

        .close-modal {
            background: #dc3545;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 13px;
        }

        .print-preview-container {
            background: #f0f0f0;
            padding: 15px;
        }

        .error-message {
            color: #dc3545;
            font-size: 11px;
            margin-top: 3px;
            display: none;
        }

        .error-message.show {
            display: block;
        }

        .btn-add-student {
            background: #0066cc;
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            margin-bottom: 15px;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            width: 100%;
            justify-content: center;
        }

        .reports-container {
            margin-top: 30px;
        }

        .alert {
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 15px;
            font-size: 13px;
        }

        .alert-info {
            background: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }

        .student-actions {
            display: flex;
            flex-direction: column;
            gap: 8px;
            margin-bottom: 15px;
        }

        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 2000;
            display: none;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            color: white;
        }

        .loading-overlay.active {
            display: flex;
        }

        .spinner {
            width: 40px;
            height: 40px;
            border: 4px solid #f3f3f3;
            border-top: 4px solid #667eea;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 15px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* PDF Status Overlay */
        .pdf-status-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.85);
            z-index: 3000;
            display: none;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            color: white;
            padding: 20px;
        }

        .pdf-status-overlay.active {
            display: flex;
        }

        .pdf-progress-container {
            width: 80%;
            max-width: 300px;
            height: 8px;
            background: rgba(255,255,255,0.2);
            border-radius: 4px;
            overflow: hidden;
            margin-top: 20px;
        }

        .pdf-progress-bar {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            width: 0%;
            transition: width 0.3s ease;
        }

        @media print {
            body {
                background: white;
                padding: 0;
            }
            .report-sheet {
                box-shadow: none;
                margin: 0;
                page-break-after: always;
                width: 210mm;
                min-height: 297mm;
                padding: 20mm;
            }
            .watermark {
                color: rgba(0, 102, 204, 0.05) !important;
                -webkit-print-color-adjust: exact;
                print-color-adjust: exact;
            }
            .no-print {
                display: none !important;
            }
            .modal-overlay {
                position: static;
                background: white;
                overflow: visible;
            }
            .modal-content {
                max-width: 100%;
                padding: 0;
            }
            .modal-header {
                display: none;
            }
            .print-preview-container {
                background: white;
                padding: 0;
            }
        }
    </style>
</head>
<body>
    <!-- PDF Status Overlay -->
    <div id="pdfStatusOverlay" class="pdf-status-overlay no-print">
        <div class="spinner"></div>
        <h3 style="margin-bottom: 10px;">Generating PDF...</h3>
        <p id="pdfStatusText">Please wait</p>
        <div class="pdf-progress-container">
            <div class="pdf-progress-bar" id="pdfProgressBar"></div>
        </div>
    </div>

    <!-- Loading Overlay -->
    <div id="loadingOverlay" class="loading-overlay">
        <div class="spinner"></div>
        <p>Processing...</p>
    </div>

    <!-- Developer Button -->
    <button class="developer-btn no-print" onclick="openDeveloperModal()">
        <span>👨‍💻</span> Developer
    </button>

    <!-- Developer Modal -->
    <div id="developerModal" class="developer-modal no-print" onclick="closeDeveloperModal(event)">
        <div class="developer-content" onclick="event.stopPropagation()">
            <button class="close-developer" onclick="closeDeveloperModal()">×</button>
            
            <div class="developer-header">
                <div class="developer-avatar">👨‍💻</div>
                <h2>Abdullahi Hashimu Kida</h2>
                <p style="color: #666; font-size: 13px;">Software Developer & Digital Solutions Expert</p>
            </div>

            <div class="developer-info">
                <h3 style="color: #667eea; margin-bottom: 12px; font-size: 15px;">About the Developer</h3>
                <p style="margin-bottom: 15px; font-size: 13px; color: #666; line-height: 1.5;">
                    Experienced software developer specializing in educational technology solutions. 
                    Founder of <strong>Kida Digital Solutions Centre</strong>, providing innovative 
                    digital tools for schools and educational institutions across Nigeria.
                </p>

                <h3 style="color: #667eea; margin-bottom: 12px; font-size: 15px;">Contact Information</h3>
                <div class="contact-grid">
                    <div class="contact-item">
                        <strong>📍 Address</strong>
                        Besides Kida Primary School,<br>Hawul L.G.A, Borno State
                    </div>
                    <div class="contact-item">
                        <strong>📞 Phone</strong>
                        0703 856 2100
                    </div>
                    <div class="contact-item">
                        <strong>📧 Email</strong>
                        support@kidadigital.com
                    </div>
                    <div class="contact-item">
                        <strong>🌐 Website</strong>
                        www.kidadigital.com
                    </div>
                </div>

                <div style="margin-top: 20px; padding: 15px; background: #f8f9fa; border-radius: 8px;">
                    <h4 style="color: #333; margin-bottom: 8px; font-size: 14px;">Services Offered</h4>
                    <ul style="list-style: none; padding: 0; color: #666; font-size: 13px; line-height: 1.8;">
                        <li>✓ School Management Systems</li>
                        <li>✓ Student Report Sheet Generators</li>
                        <li>✓ Digital Result Processing</li>
                        <li>✓ ICT Training for Schools</li>
                        <li>✓ Custom Software Development</li>
                    </ul>
                </div>

                <div style="margin-top: 15px; text-align: center; color: #999; font-size: 11px;">
                    <p>© 2026 Kida Digital Solutions Centre. All rights reserved.</p>
                    <p>Version 1.0 | Last Updated: February 2026</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Print Preview Modal -->
    <div id="printModal" class="modal-overlay no-print">
        <div class="modal-content">
            <div class="modal-header no-print">
                <h2 style="font-size: 16px;">Print Preview</h2>
                <div style="display: flex; gap: 8px;">
                    <button class="btn btn-primary btn-sm" onclick="window.print()" style="width: auto; font-size: 13px;">
                        🖨️ Print
                    </button>
                    <button class="btn btn-secondary btn-sm" onclick="closePrintModal()" style="width: auto; font-size: 13px;">
                        Close
                    </button>
                </div>
            </div>
            <div class="print-preview-container" id="printPreviewContainer"></div>
        </div>
    </div>

    <!-- Login Form -->
    <div id="loginForm" class="auth-container">
        <div class="auth-header">
            <h1>Welcome Back</h1>
            <p>Sign in to generate report sheets</p>
        </div>
        <form onsubmit="return handleLogin(event)" autocomplete="off">
            <div class="form-group">
                <label>Email Address</label>
                <input type="email" id="loginEmail" required placeholder="Enter your email" autocomplete="off">
            </div>
            <div class="form-group">
                <label>Password</label>
                <input type="password" id="loginPassword" required placeholder="Enter your password" autocomplete="off">
            </div>
            <button type="submit" class="btn btn-primary">Sign In</button>
        </form>
        <div class="auth-links">
            <a onclick="showForgotPassword()">Forgot Password?</a><br><br>
            <span>Don't have an account? </span>
            <a onclick="showRegister()">Register</a>
        </div>
    </div>

    <!-- Register Form -->
    <div id="registerForm" class="auth-container hidden">
        <div class="auth-header">
            <h1>Create Account</h1>
            <p>Register to start generating reports</p>
        </div>
        <form onsubmit="return handleRegister(event)" autocomplete="off">
            <div class="form-group">
                <label>Full Name</label>
                <input type="text" id="regName" required placeholder="Enter your full name" autocomplete="off">
            </div>
            <div class="form-group">
                <label>Email Address</label>
                <input type="email" id="regEmail" required placeholder="Enter your email" autocomplete="off">
            </div>
            <div class="form-group">
                <label>Password</label>
                <input type="password" id="regPassword" required placeholder="Create a password" minlength="6" autocomplete="off">
            </div>
            <div class="form-group">
                <label>Confirm Password</label>
                <input type="password" id="regConfirmPassword" required placeholder="Confirm your password" autocomplete="off">
            </div>
            <button type="submit" class="btn btn-primary">Register</button>
        </form>
        <div class="auth-links">
            <span>Already have an account? </span>
            <a onclick="showLogin()">Sign In</a>
        </div>
    </div>

    <!-- Forgot Password Form -->
    <div id="forgotForm" class="auth-container hidden">
        <div class="auth-header">
            <h1>Reset Password</h1>
            <p>Enter your email to receive reset instructions</p>
        </div>
        <form onsubmit="return handleForgotPassword(event)" autocomplete="off">
            <div class="form-group">
                <label>Email Address</label>
                <input type="email" id="forgotEmail" required placeholder="Enter your email" autocomplete="off">
            </div>
            <button type="submit" class="btn btn-primary">Send Reset Link</button>
        </form>
        <div class="auth-links">
            <a onclick="showLogin()">Back to Login</a>
        </div>
    </div>

    <!-- Main Application -->
    <div id="appContainer" class="app-container">
        <div class="container">
            <div class="app-header">
                <h2>📚 Report Generator</h2>
                <div class="user-info">
                    <span id="userDisplay">User</span>
                    <button class="btn btn-sm btn-secondary" onclick="logout()">Logout</button>
                </div>
            </div>

            <!-- Progress Bar -->
            <div class="progress-bar">
                <div class="progress-step active" id="step1">1</div>
                <div class="progress-step" id="step2">2</div>
                <div class="progress-step" id="step3">3</div>
            </div>

            <!-- Section 1: School Configuration -->
            <div id="section1" class="section">
                <div class="section-header">
                    <div class="section-number">1</div>
                    <h3>School Configuration</h3>
                </div>
                
                <!-- Logo Upload -->
                <div class="form-group">
                    <label>School Logo</label>
                    <div class="logo-upload" onclick="document.getElementById('logoInput').click()">
                        <input type="file" id="logoInput" accept="image/*" style="display: none;" onchange="handleLogoUpload(event)">
                        <div id="logoUploadText">
                            <div style="font-size: 36px; margin-bottom: 8px;">📷</div>
                            <div style="font-size: 13px;">Tap to upload school logo</div>
                            <div style="font-size: 11px; color: #666; margin-top: 4px;">PNG or JPG</div>
                        </div>
                        <img id="logoPreview" class="logo-preview" alt="School Logo">
                        <div id="removeLogo" class="remove-logo" onclick="removeLogo(event)">Remove Logo</div>
                    </div>
                </div>

                <div class="form-group">
                    <label>School Name</label>
                    <input type="text" id="schoolName" placeholder="e.g., Junior Secondary School Kida" required oninput="updateWatermarkText()" autocomplete="off">
                </div>
                
                <div class="form-group">
                    <label>School Address</label>
                    <input type="text" id="schoolAddress" placeholder="e.g., Kida hawul Borno State" required autocomplete="off">
                </div>
                
                <div class="form-row">
                    <div class="form-group">
                        <label>Term/Session</label>
                        <input type="text" id="termSession" placeholder="e.g., First term" required autocomplete="off">
                    </div>
                    <div class="form-group">
                        <label>Class</label>
                        <input type="text" id="className" placeholder="e.g., Jss2" required autocomplete="off">
                    </div>
                </div>
                
                <div class="form-group">
                    <label>Class Teacher Name</label>
                    <input type="text" id="teacherName" placeholder="e.g., Abdul Hameed" required autocomplete="off">
                </div>

                <div class="form-row">
                    <div class="form-group">
                        <label>Next Term Begins</label>
                        <input type="date" id="nextTermDate" required autocomplete="off">
                    </div>
                    <div class="form-group">
                        <label>Term Ended</label>
                        <input type="date" id="termEndDate" required autocomplete="off">
                    </div>
                </div>

                <div class="navigation">
                    <div></div>
                    <button type="button" class="btn btn-nav btn-next" onclick="nextSection(1)">Next →</button>
                </div>
            </div>

            <!-- Section 2: Add Subjects/Tests -->
            <div id="section2" class="section hidden">
                <div class="section-header">
                    <div class="section-number">2</div>
                    <h3>Add Subjects/Tests</h3>
                </div>

                <div class="input-group">
                    <div class="form-group" style="flex: 1;">
                        <label>Subject Name</label>
                        <input type="text" id="subjectName" placeholder="e.g., Mathematics" autocomplete="off">
                    </div>
                    <div class="form-group" style="width: 80px;">
                        <label>Max Score</label>
                        <input type="number" id="subjectMax" value="100" min="1" max="1000" style="padding: 12px 8px;" autocomplete="off">
                    </div>
                    <button type="button" class="btn btn-primary btn-sm" onclick="addSubject()" style="margin-bottom: 15px; padding: 12px;">Add</button>
                </div>

                <div class="subject-list" id="subjectList">
                    <div class="empty-state">No subjects added yet</div>
                </div>

                <div class="navigation">
                    <button type="button" class="btn btn-nav btn-prev" onclick="prevSection(2)">← Previous</button>
                    <button type="button" class="btn btn-nav btn-next" onclick="nextSection(2)" id="btnNext2" disabled>Next →</button>
                </div>
            </div>

            <!-- Section 3: Add Students & Scores -->
            <div id="section3" class="section hidden">
                <div class="section-header">
                    <div class="section-number">3</div>
                    <h3>Add Students & Scores</h3>
                </div>

                <div class="alert alert-info">
                    <strong>Note:</strong> Scores cannot exceed maximum marks.
                </div>

                <!-- Saved Lists Section -->
                <div class="saved-lists" id="savedListsSection" style="display: none;">
                    <h4 style="margin-bottom: 12px; color: #333; font-size: 14px;">📋 Saved Lists</h4>
                    <div id="savedListsContainer"></div>
                </div>

                <div class="student-actions">
                    <button type="button" class="btn-add-student" onclick="addStudentForm()">+ Add New Student</button>
                    <button type="button" class="btn-save-list" onclick="saveCurrentList()">💾 Save List</button>
                    <button type="button" class="btn btn-info btn-sm" onclick="loadSavedLists()" style="width: 100%;">📂 View Saved</button>
                </div>

                <div class="students-container" id="studentsContainer">
                    <div class="empty-state">No students added yet. Tap "Add New Student" to begin.</div>
                </div>

                <div class="navigation">
                    <button type="button" class="btn btn-nav btn-prev" onclick="prevSection(3)">← Previous</button>
                    <button type="button" class="btn btn-nav btn-success" onclick="generateReports()" id="btnGenerate" disabled>Generate Reports</button>
                </div>
            </div>

            <!-- Reports Display Section -->
            <div id="reportsSection" class="section hidden">
                <div class="generate-section">
                    <h2 style="margin-bottom: 15px; color: #333; font-size: 18px;">Reports Generated!</h2>
                    
                    <div class="action-buttons no-print">
                        <!-- PDF Button with explicit type and no form association -->
                        <button type="button" class="btn-download-all" id="pdfDownloadBtn">
                            <span>📥</span> Save as PDF
                        </button>
                        
                        <button type="button" class="btn-print-all" onclick="openPrintPreview()">
                            <span>🖨️</span> Print Preview
                        </button>
                        
                        <button type="button" class="btn btn-secondary" onclick="startNew()">
                            Start New
                        </button>
                    </div>
                </div>

                <div class="reports-container" id="reportsContainer"></div>
            </div>
        </div>
    </div>

    <script>
        // Global Data Storage
        let currentUser = null;
        let schoolData = {};
        let subjects = [];
        let students = [];
        let currentSection = 1;
        let watermarkText = '';
        let schoolLogo = null;

        // Initialize saved lists from localStorage
        let savedStudentLists = JSON.parse(localStorage.getItem('savedStudentLists') || '[]');

        // Auth Functions
        function showLogin() {
            hideAllAuthForms();
            document.getElementById('loginForm').classList.remove('hidden');
        }

        function showRegister() {
            hideAllAuthForms();
            document.getElementById('registerForm').classList.remove('hidden');
        }

        function showForgotPassword() {
            hideAllAuthForms();
            document.getElementById('forgotForm').classList.remove('hidden');
        }

        function hideAllAuthForms() {
            document.getElementById('loginForm').classList.add('hidden');
            document.getElementById('registerForm').classList.add('hidden');
            document.getElementById('forgotForm').classList.add('hidden');
        }

        function handleLogin(e) {
            if (e) {
                e.preventDefault();
                e.stopImmediatePropagation();
            }
            const email = document.getElementById('loginEmail').value;
            currentUser = { email: email, name: email.split('@')[0] };
            document.getElementById('userDisplay').textContent = currentUser.name;
            showApp();
            return false;
        }

        function handleRegister(e) {
            if (e) {
                e.preventDefault();
                e.stopImmediatePropagation();
            }
            const password = document.getElementById('regPassword').value;
            const confirmPassword = document.getElementById('regConfirmPassword').value;
            
            if (password !== confirmPassword) {
                alert('Passwords do not match!');
                return false;
            }
            
            const email = document.getElementById('regEmail').value;
            currentUser = { email: email, name: document.getElementById('regName').value };
            document.getElementById('userDisplay').textContent = currentUser.name;
            showApp();
            return false;
        }

        function handleForgotPassword(e) {
            if (e) {
                e.preventDefault();
                e.stopImmediatePropagation();
            }
            alert('Password reset link has been sent to your email!');
            showLogin();
            return false;
        }

        function logout() {
            currentUser = null;
            schoolData = {};
            subjects = [];
            students = [];
            currentSection = 1;
            watermarkText = '';
            schoolLogo = null;
            studentIdCounter = 0;
            
            document.getElementById('appContainer').style.display = 'none';
            document.querySelectorAll('.auth-container').forEach(el => el.classList.add('hidden'));
            document.getElementById('loginForm').classList.remove('hidden');
            
            document.querySelectorAll('input').forEach(input => input.value = '');
            document.getElementById('subjectList').innerHTML = '<div class="empty-state">No subjects added yet</div>';
            document.getElementById('studentsContainer').innerHTML = '<div class="empty-state">No students added yet. Tap "Add New Student" to begin.</div>';
            document.getElementById('reportsContainer').innerHTML = '';
            
            document.querySelectorAll('.progress-step').forEach((step, index) => {
                step.classList.remove('active', 'completed');
                if (index === 0) step.classList.add('active');
            });
            
            document.getElementById('section1').classList.remove('hidden');
            document.getElementById('section2').classList.add('hidden');
            document.getElementById('section3').classList.add('hidden');
            document.getElementById('reportsSection').classList.add('hidden');
            
            document.getElementById('logoPreview').src = '';
            document.getElementById('logoPreview').classList.remove('active');
            document.getElementById('logoUploadText').style.display = 'block';
            document.getElementById('removeLogo').classList.remove('active');
        }

        function showApp() {
            document.querySelectorAll('.auth-container').forEach(el => el.classList.add('hidden'));
            document.getElementById('appContainer').style.display = 'block';
        }

        // Developer Modal Functions
        function openDeveloperModal() {
            document.getElementById('developerModal').classList.add('active');
        }

        function closeDeveloperModal(event) {
            if (!event || event.target.id === 'developerModal' || event.target.classList.contains('close-developer')) {
                document.getElementById('developerModal').classList.remove('active');
            }
        }

        // Logo Functions
        function handleLogoUpload(event) {
            const file = event.target.files[0];
            if (!file) return;

            if (!file.type.startsWith('image/')) {
                alert('Please upload an image file');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                schoolLogo = e.target.result;
                document.getElementById('logoPreview').src = schoolLogo;
                document.getElementById('logoPreview').classList.add('active');
                document.getElementById('logoUploadText').style.display = 'none';
                document.getElementById('removeLogo').classList.add('active');
            };
            reader.readAsDataURL(file);
        }

        function removeLogo(event) {
            event.stopPropagation();
            schoolLogo = null;
            document.getElementById('logoInput').value = '';
            document.getElementById('logoPreview').src = '';
            document.getElementById('logoPreview').classList.remove('active');
            document.getElementById('logoUploadText').style.display = 'block';
            document.getElementById('removeLogo').classList.remove('active');
        }

        function getLogoHTML() {
            if (schoolLogo) {
                return `<img src="${schoolLogo}" class="report-logo" alt="School Logo">`;
            }
            return `<div class="report-logo-placeholder">🏫</div>`;
        }

        // Watermark Functions
        function updateWatermarkText() {
            watermarkText = document.getElementById('schoolName').value || '';
        }

        function getWatermarkHTML() {
            const text = watermarkText || schoolData.name || 'SCHOOL NAME';
            return `<div class="watermark">${text}</div>`;
        }

        // Navigation Functions
        function nextSection(section) {
            if (section === 1) {
                const required = ['schoolName', 'schoolAddress', 'termSession', 'className', 'teacherName', 'nextTermDate', 'termEndDate'];
                for (let id of required) {
                    if (!document.getElementById(id).value) {
                        alert('Please fill all required fields');
                        document.getElementById(id).focus();
                        return;
                    }
                }
                
                schoolData = {
                    name: document.getElementById('schoolName').value,
                    address: document.getElementById('schoolAddress').value,
                    term: document.getElementById('termSession').value,
                    class: document.getElementById('className').value,
                    teacher: document.getElementById('teacherName').value,
                    nextTerm: document.getElementById('nextTermDate').value,
                    termEnd: document.getElementById('termEndDate').value,
                    logo: schoolLogo
                };
                watermarkText = schoolData.name;
            }
            
            if (section === 2 && subjects.length === 0) {
                alert('Please add at least one subject');
                return;
            }

            document.getElementById(`section${section}`).classList.add('hidden');
            document.getElementById(`section${section + 1}`).classList.remove('hidden');
            
            document.getElementById(`step${section}`).classList.remove('active');
            document.getElementById(`step${section}`).classList.add('completed');
            document.getElementById(`step${section + 1}`).classList.add('active');
            
            currentSection = section + 1;

            if (section === 2) {
                updateStudentForms();
            }
        }

        function prevSection(section) {
            document.getElementById(`section${section}`).classList.add('hidden');
            document.getElementById(`section${section - 1}`).classList.remove('hidden');
            
            document.getElementById(`step${section}`).classList.remove('active');
            document.getElementById(`step${section - 1}`).classList.remove('completed');
            document.getElementById(`step${section - 1}`).classList.add('active');
            
            currentSection = section - 1;
        }

        // Subject Management
        function addSubject() {
            const name = document.getElementById('subjectName').value.trim();
            const max = parseInt(document.getElementById('subjectMax').value);
            
            if (!name) {
                alert('Please enter a subject name');
                return;
            }
            
            if (max < 1) {
                alert('Maximum score must be at least 1');
                return;
            }
            
            subjects.push({ name, max, id: Date.now() });
            document.getElementById('subjectName').value = '';
            document.getElementById('subjectMax').value = '100';
            document.getElementById('subjectName').focus();
            
            renderSubjects();
            document.getElementById('btnNext2').disabled = false;
        }

        function removeSubject(id) {
            subjects = subjects.filter(s => s.id !== id);
            renderSubjects();
            if (subjects.length === 0) {
                document.getElementById('btnNext2').disabled = true;
            }
            updateStudentForms();
        }

        function renderSubjects() {
            const container = document.getElementById('subjectList');
            if (subjects.length === 0) {
                container.innerHTML = '<div class="empty-state">No subjects added yet</div>';
                return;
            }
            
            container.innerHTML = subjects.map(sub => `
                <div class="subject-item">
                    <div class="subject-info">
                        <span class="subject-name">${sub.name}</span>
                        <span class="subject-max">Max: ${sub.max}</span>
                    </div>
                    <button type="button" class="btn-remove" onclick="removeSubject(${sub.id})">Remove</button>
                </div>
            `).join('');
        }

        // Student Management
        let studentIdCounter = 0;

        function addStudentForm(studentData = null) {
            studentIdCounter++;
            const container = document.getElementById('studentsContainer');
            
            if (container.querySelector('.empty-state')) {
                container.innerHTML = '';
            }
            
            const studentDiv = document.createElement('div');
            studentDiv.className = 'student-card';
            studentDiv.id = `student-${studentIdCounter}`;
            
            const studentName = studentData ? studentData.name : '';
            const scores = studentData ? studentData.scores : {};
            
            studentDiv.innerHTML = `
                <div class="student-header">
                    <input type="text" class="student-name-input" placeholder="Student Name" id="studentName-${studentIdCounter}" value="${studentName}" required autocomplete="off">
                    <button type="button" class="btn btn-danger btn-sm" onclick="removeStudent(${studentIdCounter})">Remove</button>
                </div>
                <div class="scores-grid" id="scores-${studentIdCounter}">
                    ${subjects.map((sub, idx) => {
                        const scoreValue = scores[sub.name] || '';
                        return `
                        <div class="score-input-group">
                            <label>${sub.name}</label>
                            <div class="score-input-wrapper">
                                <input type="number" 
                                    class="score-input" 
                                    id="score-${studentIdCounter}-${idx}" 
                                    data-max="${sub.max}"
                                    data-subject="${sub.name}"
                                    min="0" 
                                    max="${sub.max}"
                                    value="${scoreValue}"
                                    placeholder="0"
                                    oninput="validateScore(this, ${sub.max})"
                                    required autocomplete="off">
                                <span class="max-label">/ ${sub.max}</span>
                            </div>
                            <div class="error-message" id="error-${studentIdCounter}-${idx}">Score cannot exceed ${sub.max}</div>
                        </div>
                    `}).join('')}
                </div>
            `;
            
            container.appendChild(studentDiv);
            updateGenerateButton();
        }

        function removeStudent(id) {
            const element = document.getElementById(`student-${id}`);
            element.remove();
            if (document.querySelectorAll('.student-card').length === 0) {
                document.getElementById('studentsContainer').innerHTML = '<div class="empty-state">No students added yet. Tap "Add New Student" to begin.</div>';
            }
            updateGenerateButton();
        }

        function validateScore(input, max) {
            const value = parseFloat(input.value);
            const errorDiv = document.getElementById(input.id.replace('score', 'error'));
            
            if (value > max) {
                input.classList.add('error');
                errorDiv.classList.add('show');
                input.value = max;
            } else if (value < 0) {
                input.value = 0;
            } else {
                input.classList.remove('error');
                errorDiv.classList.remove('show');
            }
        }

        function updateStudentForms() {
            document.querySelectorAll('.student-card').forEach(card => {
                const studentId = card.id.split('-')[1];
                const scoresGrid = card.querySelector('.scores-grid');
                const existingInputs = scoresGrid.querySelectorAll('.score-input');
                const existingScores = {};
                
                existingInputs.forEach(input => {
                    existingScores[input.dataset.subject] = input.value;
                });
                
                scoresGrid.innerHTML = subjects.map((sub, idx) => `
                    <div class="score-input-group">
                        <label>${sub.name}</label>
                        <div class="score-input-wrapper">
                            <input type="number" 
                                class="score-input" 
                                id="score-${studentId}-${idx}" 
                                data-max="${sub.max}"
                                data-subject="${sub.name}"
                                min="0" 
                                max="${sub.max}"
                                value="${existingScores[sub.name] || ''}"
                                placeholder="0"
                                oninput="validateScore(this, ${sub.max})"
                                required autocomplete="off">
                            <span class="max-label">/ ${sub.max}</span>
                        </div>
                        <div class="error-message" id="error-${studentId}-${idx}">Score cannot exceed ${sub.max}</div>
                    </div>
                `).join('');
            });
        }

        function updateGenerateButton() {
            const hasStudents = document.querySelectorAll('.student-card').length > 0;
            document.getElementById('btnGenerate').disabled = !hasStudents;
        }

        // Save/Load Student Lists
        function saveCurrentList() {
            const studentCards = document.querySelectorAll('.student-card');
            if (studentCards.length === 0) {
                alert('No students to save!');
                return;
            }

            const listName = prompt('Enter a name for this student list:', `${schoolData.class} - ${new Date().toLocaleDateString()}`);
            if (!listName) return;

            const studentsData = [];
            studentCards.forEach(card => {
                const studentId = card.id.split('-')[1];
                const name = document.getElementById(`studentName-${studentId}`).value;
                if (!name) return;

                const scores = {};
                subjects.forEach((sub, idx) => {
                    const scoreInput = document.getElementById(`score-${studentId}-${idx}`);
                    scores[sub.name] = scoreInput.value || '';
                });

                studentsData.push({ name, scores });
            });

            const savedList = {
                id: Date.now(),
                name: listName,
                class: schoolData.class,
                subjects: subjects.map(s => ({ name: s.name, max: s.max })),
                students: studentsData,
                date: new Date().toISOString()
            };

            savedStudentLists.push(savedList);
            localStorage.setItem('savedStudentLists', JSON.stringify(savedStudentLists));
            
            alert(`Saved "${listName}" with ${studentsData.length} students!`);
            loadSavedLists();
        }

        function loadSavedLists() {
            const section = document.getElementById('savedListsSection');
            const container = document.getElementById('savedListsContainer');
            
            if (savedStudentLists.length === 0) {
                section.style.display = 'none';
                alert('No saved lists found!');
                return;
            }

            section.style.display = 'block';
            
            const relevantLists = savedStudentLists.filter(list => list.class === schoolData.class);
            
            if (relevantLists.length === 0) {
                container.innerHTML = '<p style="color: #666; font-style: italic; font-size: 13px;">No saved lists for this class.</p>';
                return;
            }

            container.innerHTML = relevantLists.map(list => `
                <div class="saved-list-item">
                    <div>
                        <strong>${list.name}</strong><br>
                        <small style="color: #666;">${list.students.length} students • ${new Date(list.date).toLocaleDateString()}</small>
                    </div>
                    <div style="display: flex; gap: 8px;">
                        <button class="btn btn-primary btn-sm" onclick="loadList(${list.id})" style="width: auto;">Load</button>
                        <button class="btn btn-danger btn-sm" onclick="deleteList(${list.id})" style="width: auto;">Delete</button>
                    </div>
                </div>
            `).join('');
        }

        function loadList(id) {
            const list = savedStudentLists.find(l => l.id === id);
            if (!list) return;

            if (!confirm(`Load "${list.name}" with ${list.students.length} students? Current students will be replaced.`)) {
                return;
            }

            document.getElementById('studentsContainer').innerHTML = '';
            
            list.students.forEach(studentData => {
                addStudentForm(studentData);
            });

            alert(`Loaded ${list.students.length} students successfully!`);
        }

        function deleteList(id) {
            if (!confirm('Are you sure you want to delete this saved list?')) return;
            
            savedStudentLists = savedStudentLists.filter(l => l.id !== id);
            localStorage.setItem('savedStudentLists', JSON.stringify(savedStudentLists));
            loadSavedLists();
        }

        // Report Generation
        function calculateGrade(percentage) {
            if (percentage >= 70) return { grade: 'A', class: 'grade-a', remark: 'Excellent' };
            if (percentage >= 60) return { grade: 'B', class: 'grade-b', remark: 'Very Good' };
            if (percentage >= 50) return { grade: 'C', class: 'grade-c', remark: 'Good' };
            if (percentage >= 45) return { grade: 'D', class: 'grade-d', remark: 'Pass' };
            if (percentage >= 40) return { grade: 'E', class: 'grade-e', remark: 'Fair' };
            return { grade: 'F', class: 'grade-f', remark: 'Fail' };
        }

        function generateReports() {
            students = [];
            const studentCards = document.querySelectorAll('.student-card');
            
            studentCards.forEach(card => {
                const studentId = card.id.split('-')[1];
                const name = document.getElementById(`studentName-${studentId}`).value;
                
                if (!name) {
                    alert('Please enter all student names');
                    return;
                }
                
                const scores = [];
                let totalScore = 0;
                let totalMax = 0;
                
                subjects.forEach((sub, idx) => {
                    const scoreInput = document.getElementById(`score-${studentId}-${idx}`);
                    const score = parseFloat(scoreInput.value) || 0;
                    const percentage = (score / sub.max) * 100;
                    const gradeInfo = calculateGrade(percentage);
                    
                    scores.push({
                        subject: sub.name,
                        score: score,
                        max: sub.max,
                        percentage: percentage.toFixed(1),
                        grade: gradeInfo.grade,
                        gradeClass: gradeInfo.class
                    });
                    
                    totalScore += score;
                    totalMax += sub.max;
                });
                
                const overallPercentage = (totalScore / totalMax) * 100;
                const overallGrade = calculateGrade(overallPercentage);
                
                students.push({
                    name,
                    scores,
                    totalScore,
                    totalMax,
                    overallPercentage: overallPercentage.toFixed(1),
                    overallGrade: overallGrade.grade,
                    overallGradeClass: overallGrade.class,
                    remark: overallGrade.remark
                });
            });
            
            students.sort((a, b) => b.overallPercentage - a.overallPercentage);
            students.forEach((student, index) => {
                student.position = index + 1;
            });
            
            renderReports();
            document.getElementById('section3').classList.add('hidden');
            document.getElementById('reportsSection').classList.remove('hidden');
            window.scrollTo(0, 0);
            
            // Attach PDF button event after reports are generated
            attachPdfButtonEvent();
        }

        function renderReports() {
            const container = document.getElementById('reportsContainer');
            container.innerHTML = '';
            
            students.forEach((student, index) => {
                const reportHTML = `
                    <div class="report-sheet" id="report-${index}">
                        ${getWatermarkHTML()}
                        <div class="report-content">
                            <div class="report-header-with-logos">
                                ${getLogoHTML()}
                                <div class="report-header-center">
                                    <div class="school-name">${schoolData.name}</div>
                                    <div class="school-address">${schoolData.address}</div>
                                    <div class="report-title">STUDENT REPORT SHEET</div>
                                    <div style="color: #666; font-size: 12px; margin-top: 4px;">${schoolData.term}</div>
                                </div>
                                ${getLogoHTML()}
                            </div>
                            
                            <div class="student-info-grid">
                                <div class="info-item">
                                    <span class="info-label">Student Name</span>
                                    <span class="info-value">${student.name}</span>
                                </div>
                                <div class="info-item">
                                    <span class="info-label">Class</span>
                                    <span class="info-value">${schoolData.class}</span>
                                </div>
                                <div class="info-item">
                                    <span class="info-label">Position</span>
                                    <span class="info-value" style="color: #dc3545;">#${student.position} of ${students.length}</span>
                                </div>
                            </div>
                            
                            <table class="results-table">
                                <thead>
                                    <tr>
                                        <th>SUBJECT</th>
                                        <th>SCORE</th>
                                        <th>MAX</th>
                                        <th>%</th>
                                        <th>GRD</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    ${student.scores.map(s => `
                                        <tr>
                                            <td>${s.subject}</td>
                                            <td>${s.score}</td>
                                            <td>${s.max}</td>
                                            <td>${s.percentage}%</td>
                                            <td><span class="grade ${s.gradeClass}">${s.grade}</span></td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                            </table>
                            
                            <div class="summary-box">
                                <div class="summary-item">
                                    <div class="summary-label">Total</div>
                                    <div class="summary-value">${student.totalScore}/${student.totalMax}</div>
                                </div>
                                <div class="summary-item">
                                    <div class="summary-label">Average</div>
                                    <div class="summary-value">${student.overallPercentage}%</div>
                                </div>
                                <div class="summary-item">
                                    <div class="summary-label">Grade</div>
                                    <div class="summary-grade ${student.overallGradeClass}">${student.overallGrade}</div>
                                </div>
                            </div>
                            
                            <div class="remarks">
                                ${student.remark} - ${student.overallPercentage >= 50 ? 'PROMOTED' : 'NOT PROMOTED'}
                            </div>
                            
                            <div class="term-info">
                                <strong>Term Information:</strong><br>
                                Next Term: ${new Date(schoolData.nextTerm).toLocaleDateString()}<br>
                                Term Ended: ${new Date(schoolData.termEnd).toLocaleDateString()}<br>
                                Total Students: ${students.length}
                            </div>
                            
                            <div class="signatures">
                                <div class="signature-line">
                                    <div class="line">${schoolData.teacher}</div>
                                    <div class="title">Class Teacher<br>Signature & Date</div>
                                </div>
                                <div class="signature-line">
                                    <div class="line"></div>
                                    <div class="title">Principal<br>Signature & Date</div>
                                </div>
                                <div class="signature-line">
                                    <div class="line"></div>
                                    <div class="title">Parent/Guardian<br>Signature & Date</div>
                                </div>
                            </div>
                            
                            <div style="text-align: center; margin-top: 20px; font-size: 10px; color: #999;">
                                Generated on ${new Date().toLocaleDateString()} | ${schoolData.name}
                            </div>
                        </div>
                    </div>
                `;
                
                container.innerHTML += reportHTML;
            });
        }

        // Print Preview Functions
        function openPrintPreview() {
            const modal = document.getElementById('printModal');
            const container = document.getElementById('printPreviewContainer');
            const reports = document.querySelectorAll('.report-sheet');
            
            container.innerHTML = '';
            reports.forEach(report => {
                const clone = report.cloneNode(true);
                container.appendChild(clone);
            });
            
            modal.classList.add('active');
            document.body.style.overflow = 'hidden';
        }

        function closePrintModal() {
            const modal = document.getElementById('printModal');
            modal.classList.remove('active');
            document.body.style.overflow = '';
        }

        // PDF Generation - Fixed for AppCreator24
        function attachPdfButtonEvent() {
            const pdfBtn = document.getElementById('pdfDownloadBtn');
            if (pdfBtn) {
                // Remove any existing listeners
                pdfBtn.replaceWith(pdfBtn.cloneNode(true));
                
                // Get fresh reference and attach event
                const newPdfBtn = document.getElementById('pdfDownloadBtn');
                newPdfBtn.addEventListener('click', function(e) {
                    e.preventDefault();
                    e.stopPropagation();
                    e.stopImmediatePropagation();
                    generatePDF();
                    return false;
                }, false);
            }
        }

        async function generatePDF() {
            const statusOverlay = document.getElementById('pdfStatusOverlay');
            const progressBar = document.getElementById('pdfProgressBar');
            const statusText = document.getElementById('pdfStatusText');
            
            try {
                statusOverlay.classList.add('active');
                progressBar.style.width = '10%';
                statusText.textContent = 'Preparing...';
                
                const reports = document.querySelectorAll('.report-sheet');
                if (reports.length === 0) {
                    alert('No reports to download!');
                    statusOverlay.classList.remove('active');
                    return;
                }
                
                await new Promise(resolve => setTimeout(resolve, 100));
                progressBar.style.width = '30%';
                statusText.textContent = 'Processing reports...';
                
                // Create processing container
                const processDiv = document.createElement('div');
                processDiv.style.cssText = 'position:fixed;left:-9999px;top:0;width:210mm;background:white;';
                document.body.appendChild(processDiv);
                
                // Clone reports
                reports.forEach(report => {
                    const clone = report.cloneNode(true);
                    clone.style.cssText = 'width:210mm;min-height:297mm;padding:20mm;page-break-after:always;';
                    processDiv.appendChild(clone);
                });
                
                await new Promise(resolve => setTimeout(resolve, 100));
                progressBar.style.width = '60%';
                statusText.textContent = 'Generating PDF...';
                
                // Use html2canvas
                const canvas = await html2canvas(processDiv, {
                    scale: 2,
                    useCORS: true,
                    allowTaint: true,
                    backgroundColor: '#ffffff',
                    logging: false,
                    width: 794,
                    height: processDiv.scrollHeight
                });
                
                progressBar.style.width = '80%';
                statusText.textContent = 'Saving...';
                
                // Create PDF
                const { jsPDF } = window.jspdf;
                const pdf = new jsPDF('p', 'mm', 'a4');
                
                const imgData = canvas.toDataURL('image/jpeg', 0.95);
                const imgWidth = 210;
                const pageHeight = 297;
                const imgHeight = (canvas.height * imgWidth) / canvas.width;
                let heightLeft = imgHeight;
                let position = 0;
                
                pdf.addImage(imgData, 'JPEG', 0, position, imgWidth, imgHeight);
                heightLeft -= pageHeight;
                
                while (heightLeft > 0) {
                    position = heightLeft - imgHeight;
                    pdf.addPage();
                    pdf.addImage(imgData, 'JPEG', 0, position, imgWidth, imgHeight);
                    heightLeft -= pageHeight;
                }
                
                // Add page numbers
                const totalPages = pdf.internal.getNumberOfPages();
                for (let i = 1; i <= totalPages; i++) {
                    pdf.setPage(i);
                    pdf.setFontSize(10);
                    pdf.setTextColor(150);
                    pdf.text(`Page ${i} of ${totalPages}`, 105, 290, { align: 'center' });
                }
                
                // Save
                const filename = `Reports_${schoolData.class}_${schoolData.term}_${new Date().getFullYear()}.pdf`;
                pdf.save(filename);
                
                // Cleanup
                document.body.removeChild(processDiv);
                
                progressBar.style.width = '100%';
                statusText.textContent = 'Complete!';
                
                setTimeout(() => {
                    statusOverlay.classList.remove('active');
                    progressBar.style.width = '0%';
                }, 1000);
                
            } catch (error) {
                console.error('PDF Error:', error);
                statusOverlay.classList.remove('active');
                alert('PDF generation failed. Please use Print Preview and select "Save as PDF" from the print dialog.');
            }
        }

        function startNew() {
            if (confirm('Start new report? All current data will be cleared.')) {
                location.reload();
            }
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            const today = new Date();
            const nextMonth = new Date(today.getFullYear(), today.getMonth() + 1, today.getDate());
            document.getElementById('termEndDate').valueAsDate = today;
            document.getElementById('nextTermDate').valueAsDate = nextMonth;
            
            document.addEventListener('keydown', function(e) {
                if (e.key === 'Escape') {
                    closePrintModal();
                    closeDeveloperModal();
                }
            });
        });
    </script>
</body>
</html>
