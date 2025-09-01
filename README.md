<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inovarte - Sistema de Ordens de Serviço</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --main-blue: #1e88e5;
            --dark-blue: #0d47a1;
            --light-blue: #bbdefb;
            --card-blue: #e3f2fd;
        }
        
        body {
            background-color: #f5f9fc;
            color: #333;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        .navbar, .btn-primary {
            background-color: var(--main-blue) !important;
        }
        
        .main-content {
            padding: 20px;
            margin-top: 80px;
        }
        
        .card {
            border: none;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        
        .card:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
        }
        
        .card-header {
            background-color: var(--card-blue);
            font-weight: bold;
            border-radius: 8px 8px 0 0 !important;
        }
        
        .kanban-container {
            display: flex;
            overflow-x: auto;
            gap: 15px;
            padding: 10px 0;
        }
        
        .kanban-column {
            min-width: 300px;
            min-height: 600px;
            background-color: #f0f8ff;
            border-radius: 8px;
            padding: 15px;
        }
        
        .kanban-card {
            margin-bottom: 15px;
            cursor: grab;
            border-left: 5px solid #ccc;
            transition: transform 0.2s;
            border-radius: 6px;
        }
        
        .kanban-card:active {
            cursor: grabbing;
            transform: rotate(3deg);
        }
        
        .priority-low {
            border-left-color: #4caf50 !important;
        }
        
        .priority-medium {
            border-left-color: #ff9800 !important;
        }
        
        .priority-high {
            border-left-color: #f44336 !important;
        }
        
        .calendar-day {
            cursor: pointer;
            transition: all 0.3s;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 2px auto;
        }
        
        .calendar-day:hover {
            background-color: var(--light-blue);
        }
        
        .has-orders {
            background-color: var(--main-blue);
            color: white;
            font-weight: bold;
        }
        
        .login-container {
            max-width: 400px;
            margin: 100px auto;
            padding: 30px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
        }
        
        .dragging {
            opacity: 0.7;
            background-color: var(--light-blue);
        }
        
        .order-badge {
            font-size: 0.75rem;
        }
        
        .order-actions a {
            text-decoration: none;
            margin-right: 10px;
        }
        
        .image-thumbnail {
            width: 100%;
            height: 100px;
            object-fit: cover;
            border-radius: 5px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .image-thumbnail:hover {
            transform: scale(1.05);
        }
        
        .modal-image {
            width: 100%;
            border-radius: 5px;
        }
        
        .image-gallery {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 10px;
        }
        
        .gallery-item {
            width: 100px;
            height: 100px;
            position: relative;
        }
        
        .delete-image {
            position: absolute;
            top: 5px;
            right: 5px;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 50%;
            width: 20px;
            height: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
        }
        
        .dashboard-card {
            text-align: center;
            padding: 20px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .dashboard-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }
        
        .dashboard-icon {
            font-size: 2.5rem;
            margin-bottom: 15px;
        }
        
        .dashboard-number {
            font-size: 2rem;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .dashboard-label {
            font-size: 1rem;
            color: #6c757d;
        }
        
        .orders-table {
            width: 100%;
        }
        
        .orders-table th {
            background-color: var(--card-blue);
        }
        
        .status-badge {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 500;
        }
        
        .table-hover tbody tr:hover {
            background-color: rgba(30, 136, 229, 0.1);
        }
        
        .nav-link {
            color: rgba(255, 255, 255, 0.85);
            transition: color 0.3s;
        }
        
        .nav-link:hover, .nav-link.active {
            color: white;
        }
        
        .delete-order {
            color: #dc3545;
            cursor: pointer;
        }
        
        .delete-order:hover {
            color: #bd2130;
        }
        
        .toast-container {
            position: fixed;
            top: 100px;
            right: 20px;
            z-index: 9999;
        }
        
        .toast {
            box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
        }
    </style>
</head>
<body>
    <!-- Sistema de Login -->
    <div id="login-page" class="container-fluid">
        <div class="login-container">
            <h2 class="text-center mb-4" style="color: var(--main-blue);">
                <i class="fas fa-print me-2"></i>Inovarte
            </h2>
            <p class="text-center mb-4">Sistema de Gestão de Ordens de Serviço</p>
            <form id="login-form">
                <div class="mb-3">
                    <label for="username" class="form-label">Usuário</label>
                    <input type="text" class="form-control" id="username" required>
                </div>
                <div class="mb-3">
                    <label for="password" class="form-label">Senha</label>
                    <input type="password" class="form-control" id="password" required>
                </div>
                <button type="submit" class="btn btn-primary w-100">Entrar</button>
            </form>
            <div id="login-error" class="alert alert-danger mt-3 d-none">
                Usuário ou senha incorretos!
            </div>
            <p class="text-center mt-3 small">Use admin/admin para acessar como administrador</p>
        </div>
    </div>

    <!-- Sistema Principal (inicialmente oculto) -->
    <div id="app-container" class="d-none">
        <!-- Navbar Superior -->
        <nav class="navbar navbar-expand-lg navbar-dark fixed-top">
            <div class="container-fluid">
                <a class="navbar-brand" href="#">
                    <i class="fas fa-print me-2"></i>Inovarte
                </a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarNav">
                    <ul class="navbar-nav me-auto">
                        <li class="nav-item">
                            <a class="nav-link active" href="#" id="home-link"><i class="fas fa-home me-1"></i> Dashboard</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" id="new-os-link"><i class="fas fa-plus-circle me-1"></i> Nova OS</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" id="calendar-link"><i class="fas fa-calendar-alt me-1"></i> Agenda</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" id="orders-link"><i class="fas fa-list me-1"></i> Todas as OS</a>
                        </li>
                        <li class="nav-item admin-only d-none">
                            <a class="nav-link" href="#" id="users-link"><i class="fas fa-users me-1"></i> Usuários</a>
                        </li>
                    </ul>
                    <ul class="navbar-nav">
                        <li class="nav-item">
                            <a class="nav-link" href="#" id="logout-btn"><i class="fas fa-sign-out-alt me-1"></i> Sair</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>

        <!-- Container para mensagens toast -->
        <div class="toast-container">
            <div id="successToast" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
                <div class="toast-header bg-success text-white">
                    <strong class="me-auto">Sucesso</strong>
                    <button type="button" class="btn-close btn-close-white" data-bs-dismiss="toast" aria-label="Close"></button>
                </div>
                <div class="toast-body">
                    Operação realizada com sucesso!
                </div>
            </div>
        </div>

        <div class="container-fluid main-content">
            <!-- Página Inicial - Dashboard -->
            <div id="home-page">
                <div class="d-flex justify-content-between align-items-center mb-4">
                    <h2>Dashboard</h2>
                    <span id="welcome-user" style="color: var(--main-blue);"></span>
                </div>
                
                <div class="row mb-5">
                    <div class="col-md-3">
                        <div class="card dashboard-card" data-status="Aguardando">
                            <div class="card-body">
                                <div class="dashboard-icon text-primary">
                                    <i class="fas fa-clock"></i>
                                </div>
                                <div class="dashboard-number text-primary" id="count-aguardando">0</div>
                                <div class="dashboard-label">Aguardando</div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="card dashboard-card" data-status="Em execução">
                            <div class="card-body">
                                <div class="dashboard-icon text-warning">
                                    <i class="fas fa-cogs"></i>
                                </div>
                                <div class="dashboard-number text-warning" id="count-execucao">0</div>
                                <div class="dashboard-label">Em Execução</div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="card dashboard-card" data-status="Atrasado">
                            <div class="card-body">
                                <div class="dashboard-icon text-danger">
                                    <i class="fas fa-exclamation-triangle"></i>
                                </div>
                                <div class="dashboard-number text-danger" id="count-atrasado">0</div>
                                <div class="dashboard-label">Atrasados</div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="card dashboard-card" data-status="Concluído">
                            <div class="card-body">
                                <div class="dashboard-icon text-success">
                                    <i class="fas fa-check-circle"></i>
                                </div>
                                <div class="dashboard-number text-success" id="count-concluido">0</div>
                                <div class="dashboard-label">Concluídos</div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="row">
                    <div class="col-md-8">
                        <div class="card">
                            <div class="card-header">
                                <h5 class="mb-0">Visão Geral das Ordens de Serviço</h5>
                            </div>
                            <div class="card-body">
                                <canvas id="orders-chart" height="250"></canvas>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="card">
                            <div class="card-header">
                                <h5 class="mb-0">Distribuição por Prioridade</h5>
                            </div>
                            <div class="card-body">
                                <canvas id="priority-chart" height="250"></canvas>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Formulário de Nova OS -->
            <div id="new-os-page" class="d-none">
                <h2 class="mb-4" id="os-form-title">Nova Ordem de Serviço</h2>
                
                <div class="card">
                    <div class="card-header">
                        Dados do Serviço
                    </div>
                    <div class="card-body">
                        <form id="os-form">
                            <input type="hidden" id="edit-id" value="">
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <label for="client" class="form-label">Cliente *</label>
                                    <input type="text" class="form-control" id="client" required>
                                </div>
                                <div class="col-md-6">
                                    <label for="whatsapp" class="form-label">WhatsApp *</label>
                                    <input type="tel" class="form-control" id="whatsapp" required>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-12">
                                    <label for="address" class="form-label">Endereço *</label>
                                    <input type="text" class="form-control" id="address" required>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-12">
                                    <label for="service" class="form-label">Descrição do Serviço *</label>
                                    <textarea class="form-control" id="service" rows="3" required></textarea>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-12">
                                    <label for="observation" class="form-label">Observação</label>
                                    <textarea class="form-control" id="observation" rows="2"></textarea>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <label class="form-label">Situação *</label>
                                    <select class="form-select" id="status" required>
                                        <option value="Aguardando">Aguardando</option>
                                        <option value="Impresso">Impresso</option>
                                        <option value="Cortado">Cortado</option>
                                        <option value="Embalado">Embalado</option>
                                        <option value="Em execução">Em execução</option>
                                        <option value="Atrasado">Atrasado</option>
                                        <option value="Concluído">Concluído</option>
                                    </select>
                                </div>
                                <div class="col-md-6">
                                    <label class="form-label">Prioridade *</label>
                                    <div>
                                        <div class="form-check form-check-inline">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-low" value="low" checked>
                                            <label class="form-check-label" for="priority-low">Verde (Normal)</label>
                                        </div>
                                        <div class="form-check form-check-inline">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-medium" value="medium">
                                            <label class="form-check-label" for="priority-medium">Amarelo (Importante)</label>
                                        </div>
                                        <div class="form-check form-check-inline">
                                            <input class="form-check-input" type="radio" name="priority" id="priority-high" value="high">
                                            <label class="form-check-label" for="priority-high">Vermelho (Urgente)</label>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-6">
                                    <label for="layout" class="form-label">Imagens do Layout *</label>
                                    <input type="file" class="form-control" id="layout" accept="image/*" multiple required>
                                    <div class="form-text">Selecione uma ou mais imagens</div>
                                </div>
                                <div class="col-md-6">
                                    <label for="date" class="form-label">Data de Entrega *</label>
                                    <input type="date" class="form-control" id="date" required>
                                </div>
                            </div>
                            
                            <div class="row mb-3">
                                <div class="col-md-12">
                                    <label class="form-label">Imagens Atuais</label>
                                    <div id="current-images" class="image-gallery">
                                        <!-- Imagens atuais serão exibidas aqui durante a edição -->
                                    </div>
                                </div>
                            </div>
                            
                            <div class="d-flex justify-content-end">
                                <button type="button" id="cancel-os" class="btn btn-secondary me-2">Cancelar</button>
                                <button type="submit" class="btn btn-primary" id="save-os-btn">Salvar OS</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>

            <!-- Calendário -->
            <div id="calendar-page" class="d-none">
                <h2 class="mb-4">Agenda</h2>
                
                <div class="card">
                    <div class="card-header d-flex justify-content-between align-items-center">
                        <div>
                            <button class="btn btn-sm btn-outline-primary me-2" id="prev-month">
                                <i class="fas fa-chevron-left"></i>
                            </button>
                            <span id="current-month">Junho 2023</span>
                            <button class="btn btn-sm btn-outline-primary ms-2" id="next-month">
                                <i class="fas fa-chevron-right"></i>
                            </button>
                        </div>
                        <button class="btn btn-sm btn-outline-secondary" id="today-btn">Hoje</button>
                    </div>
                    <div class="card-body">
                        <div id="calendar" class="table-responsive">
                            <!-- O calendário será gerado via JavaScript -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Quadro Kanban -->
            <div id="kanban-page" class="d-none">
                <div class="d-flex justify-content-between align-items-center mb-4">
                    <h2 id="kanban-date">Ordens de Serviço - 01/06/2023</h2>
                    <button class="btn btn-secondary" id="back-to-calendar">
                        <i class="fas fa-arrow-left me-1"></i> Voltar para Agenda
                    </button>
                </div>
                
                <div class="kanban-container">
                    <div class="kanban-column">
                        <h5 class="text-center">Aguardando</h5>
                        <div id="column-aguardando" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Impresso</h5>
                        <div id="column-impresso" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Cortado</h5>
                        <div id="column-cortado" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Embalado</h5>
                        <div id="column-embalado" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Em execução</h5>
                        <div id="column-emexecucao" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Atrasado</h5>
                        <div id="column-atrasado" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                    <div class="kanban-column">
                        <h5 class="text-center">Concluído</h5>
                        <div id="column-concluido" class="kanban-column-cards">
                            <!-- Cards serão inseridos aqui -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Lista de Todas as OS -->
            <div id="orders-page" class="d-none">
                <div class="d-flex justify-content-between align-items-center mb-4">
                    <h2>Todas as Ordens de Serviço</h2>
                    <div>
                        <button class="btn btn-outline-primary me-2" id="filter-btn">
                            <i class="fas fa-filter me-1"></i> Filtrar
                        </button>
                        <button class="btn btn-primary" id="new-order-btn">
                            <i class="fas fa-plus me-1"></i> Nova OS
                        </button>
                    </div>
                </div>
                
                <div class="card mb-4 d-none" id="filter-panel">
                    <div class="card-body">
                        <div class="row">
                            <div class="col-md-3">
                                <label for="filter-status" class="form-label">Status</label>
                                <select class="form-select" id="filter-status">
                                    <option value="">Todos</option>
                                    <option value="Aguardando">Aguardando</option>
                                    <option value="Impresso">Impresso</option>
                                    <option value="Cortado">Cortado</option>
                                    <option value="Embalado">Embalado</option>
                                    <option value="Em execução">Em execução</option>
                                    <option value="Atrasado">Atrasado</option>
                                    <option value="Concluído">Concluído</option>
                                </select>
                            </div>
                            <div class="col-md-3">
                                <label for="filter-priority" class="form-label">Prioridade</label>
                                <select class="form-select" id="filter-priority">
                                    <option value="">Todas</option>
                                    <option value="low">Normal (Verde)</option>
                                    <option value="medium">Importante (Amarelo)</option>
                                    <option value="high">Urgente (Vermelho)</option>
                                </select>
                            </div>
                            <div class="col-md-3">
                                <label for="filter-date" class="form-label">Data</label>
                                <input type="date" class="form-control" id="filter-date">
                            </div>
                            <div class="col-md-3">
                                <label class="form-label d-block">&nbsp;</label>
                                <button class="btn btn-secondary w-100" id="clear-filters">
                                    <i class="fas fa-times me-1"></i> Limpar
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="card">
                    <div class="card-body">
                        <div class="table-responsive">
                            <table class="table table-hover orders-table">
                                <thead>
                                    <tr>
                                        <th>ID</th>
                                        <th>Cliente</th>
                                        <th>Serviço</th>
                                        <th>Data</th>
                                        <th>Status</th>
                                        <th>Prioridade</th>
                                        <th>Ações</th>
                                    </tr>
                                </thead>
                                <tbody id="orders-table-body">
                                    <!-- Ordens serão listadas aqui -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Gerenciamento de Usuários -->
            <div id="users-page" class="d-none">
                <h2 class="mb-4">Gerenciamento de Usuários</h2>
                
                <div class="card mb-4">
                    <div class="card-header">
                        Adicionar Novo Usuário
                    </div>
                    <div class="card-body">
                        <form id="user-form">
                            <div class="row mb-3">
                                <div class="col-md-4">
                                    <label for="new-username" class="form-label">Usuário *</label>
                                    <input type="text" class="form-control" id="new-username" required>
                                </div>
                                <div class="col-md-4">
                                    <label for="new-password" class="form-label">Senha *</label>
                                    <input type="password" class="form-control" id="new-password" required>
                                </div>
                                <div class="col-md-4">
                                    <label for="user-role" class="form-label">Tipo de Usuário *</label>
                                    <select class="form-select" id="user-role" required>
                                        <option value="user">Usuário</option>
                                        <option value="admin">Administrador</option>
                                    </select>
                                </div>
                            </div>
                            <div class="d-flex justify-content-end">
                                <button type="submit" class="btn btn-primary">Adicionar Usuário</button>
                            </div>
                        </form>
                    </div>
                </div>
                
                <div class="card">
                    <div class="card-header">
                        Usuários do Sistema
                    </div>
                    <div class="card-body">
                        <table class="table table-striped">
                            <thead>
                                <tr>
                                    <th>Usuário</th>
                                    <th>Tipo</th>
                                    <th>Ações</th>
                                </tr>
                            </thead>
                            <tbody id="users-table">
                                <!-- Usuários serão listados aqui -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal para visualização de imagem -->
    <div class="modal fade" id="imageModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Visualização de Imagem</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body text-center">
                    <img src="" class="modal-image" id="modal-image" alt="Imagem do layout">
                </div>
            </div>
        </div>
    </div>

    <!-- Modal de confirmação de exclusão -->
    <div class="modal fade" id="deleteModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Confirmar Exclusão</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <p>Tem certeza que deseja excluir esta ordem de serviço? Esta ação não pode ser desfeita.</p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                    <button type="button" class="btn btn-danger" id="confirm-delete">Excluir</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // Dados iniciais
        const initialUsers = [
            { username: 'admin', password: 'admin', role: 'admin' },
            { username: 'usuario', password: '1234', role: 'user' }
        ];
        
        const initialOrders = [
            {
                id: 1,
                client: "Cliente Exemplo",
                address: "Rua das Flores, 123",
                whatsapp: "5511999999999",
                service: "Impressão de banners",
                observation: "Entregar até às 14h",
                status: "Aguardando",
                priority: "medium",
                date: "2023-06-15",
                images: [
                    "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='100' viewBox='0 0 100 100'%3E%3Crect width='100' height='100' fill='%23ddd'/%3E%3Ctext x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' font-family='sans-serif' font-size='12' fill='%23333'%3EImagem%20do%20Layout%3C/text%3E%3C/svg%3E"
                ]
            },
            {
                id: 2,
                client: "Outro Cliente",
                address: "Avenida Principal, 456",
                whatsapp: "5511888888888",
                service: "Cartões de visita",
                observation: "Papel premium",
                status: "Em execução",
                priority: "high",
                date: "2023-06-18",
                images: [
                    "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='100' viewBox='0 0 100 100'%3E%3Crect width='100' height='100' fill='%23eef'/%3E%3Ctext x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' font-family='sans-serif' font-size='12' fill='%23333'%3ECart%C3%A3o%20de%20Visita%3C/text%3E%3C/svg%3E"
                ]
            },
            {
                id: 3,
                client: "Cliente Concluído",
                address: "Praça Central, 789",
                whatsapp: "5511777777777",
                service: "Panfletos promocionais",
                observation: "Entrega urgente",
                status: "Concluído",
                priority: "high",
                date: "2023-06-10",
                images: [
                    "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='100' viewBox='0 0 100 100'%3E%3Crect width='100' height='100' fill='%23efe'/%3E%3Ctext x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' font-family='sans-serif' font-size='12' fill='%23333'%3EPanfleto%20Promocional%3C/text%3E%3C/svg%3E"
                ]
            }
        ];
        
        // Inicialização
        let currentUser = null;
        let users = JSON.parse(localStorage.getItem('users')) || initialUsers;
        let orders = JSON.parse(localStorage.getItem('orders')) || initialOrders;
        let currentDate = new Date();
        let selectedDate = null;
        let draggedCard = null;
        let editingOrderId = null;
        let ordersChart = null;
        let priorityChart = null;
        let orderToDelete = null;
        let successToast = null;
        
        // Salvar dados no localStorage
        function saveData() {
            localStorage.setItem('users', JSON.stringify(users));
            localStorage.setItem('orders', JSON.stringify(orders));
        }
        
        // Inicializar a aplicação
        function initApp() {
            // Verificar se há usuário logado
            const loggedUser = localStorage.getItem('loggedUser');
            if (loggedUser) {
                currentUser = JSON.parse(loggedUser);
                showApp();
            }
            
            // Inicializar toast
            successToast = new bootstrap.Toast(document.getElementById('successToast'));
            
            // Configurar evento de login
            document.getElementById('login-form').addEventListener('submit', function(e) {
                e.preventDefault();
                login();
            });
            
            // Configurar logout
            document.getElementById('logout-btn').addEventListener('click', logout);
            
            // Configurar navegação
            document.getElementById('home-link').addEventListener('click', () => {
                showPage('home-page');
                updateDashboard();
            });
            document.getElementById('new-os-link').addEventListener('click', () => {
                resetOSForm();
                showPage('new-os-page');
            });
            document.getElementById('calendar-link').addEventListener('click', () => {
                showPage('calendar-page');
                renderCalendar();
            });
            document.getElementById('orders-link').addEventListener('click', () => {
                showPage('orders-page');
                renderOrdersTable();
            });
            document.getElementById('users-link').addEventListener('click', () => {
                showPage('users-page');
                renderUsersTable();
            });
            
            // Botões da página de ordens
            document.getElementById('new-order-btn').addEventListener('click', () => {
                resetOSForm();
                showPage('new-os-page');
            });
            document.getElementById('filter-btn').addEventListener('click', () => {
                document.getElementById('filter-panel').classList.toggle('d-none');
            });
            document.getElementById('clear-filters').addEventListener('click', () => {
                document.getElementById('filter-status').value = '';
                document.getElementById('filter-priority').value = '';
                document.getElementById('filter-date').value = '';
                renderOrdersTable();
            });
            
            // Filtros
            document.getElementById('filter-status').addEventListener('change', renderOrdersTable);
            document.getElementById('filter-priority').addEventListener('change', renderOrdersTable);
            document.getElementById('filter-date').addEventListener('change', renderOrdersTable);
            
            // Formulário de OS
            document.getElementById('os-form').addEventListener('submit', function(e) {
                e.preventDefault();
                saveOS();
            });
            document.getElementById('cancel-os').addEventListener('click', () => showPage('home-page'));
            
            // Calendário
            document.getElementById('prev-month').addEventListener('click', () => {
                currentDate.setMonth(currentDate.getMonth() - 1);
                renderCalendar();
            });
            document.getElementById('next-month').addEventListener('click', () => {
                currentDate.setMonth(currentDate.getMonth() + 1);
                renderCalendar();
            });
            document.getElementById('today-btn').addEventListener('click', () => {
                currentDate = new Date();
                renderCalendar();
            });
            
            // Kanban
            document.getElementById('back-to-calendar').addEventListener('click', () => {
                showPage('calendar-page');
                renderCalendar();
            });
            
            // Usuários
            document.getElementById('user-form').addEventListener('submit', function(e) {
                e.preventDefault();
                addUser();
            });
            
            // Dashboard cards
            document.querySelectorAll('.dashboard-card').forEach(card => {
                card.addEventListener('click', function() {
                    const status = this.getAttribute('data-status');
                    showPage('orders-page');
                    document.getElementById('filter-status').value = status;
                    renderOrdersTable();
                });
            });
            
            // Modal de imagem
            const imageModal = new bootstrap.Modal(document.getElementById('imageModal'));
            document.getElementById('imageModal').addEventListener('show.bs.modal', function(event) {
                const button = event.relatedTarget;
                const imageSrc = button.getAttribute('data-image-src');
                document.getElementById('modal-image').src = imageSrc;
            });
            
            // Modal de exclusão
            const deleteModal = new bootstrap.Modal(document.getElementById('deleteModal'));
            document.getElementById('confirm-delete').addEventListener('click', function() {
                if (orderToDelete) {
                    deleteOrder(orderToDelete);
                    deleteModal.hide();
                }
            });
        }
        
        // Função de login
        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            const user = users.find(u => u.username === username && u.password === password);
            
            if (user) {
                currentUser = user;
                localStorage.setItem('loggedUser', JSON.stringify(user));
                showApp();
                document.getElementById('login-error').classList.add('d-none');
            } else {
                document.getElementById('login-error').classList.remove('d-none');
            }
        }
        
        // Função de logout
        function logout() {
            currentUser = null;
            localStorage.removeItem('loggedUser');
            document.getElementById('app-container').classList.add('d-none');
            document.getElementById('login-page').classList.remove('d-none');
            document.getElementById('login-form').reset();
        }
        
        // Mostrar aplicação
        function showApp() {
            document.getElementById('login-page').classList.add('d-none');
            document.getElementById('app-container').classList.remove('d-none');
            document.getElementById('welcome-user').textContent = `Olá, ${currentUser.username}!`;
            
            // Mostrar/ocultar funcionalidades de admin
            const adminElements = document.querySelectorAll('.admin-only');
            adminElements.forEach(el => {
                if (currentUser.role === 'admin') {
                    el.classList.remove('d-none');
                } else {
                    el.classList.add('d-none');
                }
            });
            
            showPage('home-page');
            updateDashboard();
        }
        
        // Mostrar página específica
        function showPage(pageId) {
            // Ocultar todas as páginas
            const pages = document.querySelectorAll('[id$="-page"]');
            pages.forEach(page => page.classList.add('d-none'));
            
            // Atualar navegação ativa
            document.querySelectorAll('.nav-link').forEach(link => {
                link.classList.remove('active');
            });
            
            // Ativar link correspondente
            const activeLink = document.querySelector(`[id="${pageId}-link"]`);
            if (activeLink) {
                activeLink.classList.add('active');
            }
            
            // Mostrar a página solicitada
            document.getElementById(pageId).classList.remove('d-none');
        }
        
        // Mostrar mensagem de sucesso
        function showSuccessMessage(message) {
            document.querySelector('.toast-body').textContent = message;
            successToast.show();
        }
        
        // Atualizar dashboard
        function updateDashboard() {
            // Contar ordens por status
            const statusCounts = {
                'Aguardando': orders.filter(o => o.status === 'Aguardando').length,
                'Em execução': orders.filter(o => o.status === 'Em execução').length,
                'Atrasado': orders.filter(o => o.status === 'Atrasado').length,
                'Concluído': orders.filter(o => o.status === 'Concluído').length
            };
            
            // Atualizar contadores
            document.getElementById('count-aguardando').textContent = statusCounts['Aguardando'];
            document.getElementById('count-execucao').textContent = statusCounts['Em execução'];
            document.getElementById('count-atrasado').textContent = statusCounts['Atrasado'];
            document.getElementById('count-concluido').textContent = statusCounts['Concluído'];
            
            // Criar gráfico de ordens por status
            const statusLabels = ['Aguardando', 'Impresso', 'Cortado', 'Embalado', 'Em execução', 'Atrasado', 'Concluído'];
            const statusData = statusLabels.map(status => orders.filter(o => o.status === status).length);
            
            if (ordersChart) {
                ordersChart.destroy();
            }
            
            const ordersCtx = document.getElementById('orders-chart').getContext('2d');
            ordersChart = new Chart(ordersCtx, {
                type: 'bar',
                data: {
                    labels: statusLabels,
                    datasets: [{
                        label: 'Ordens por Status',
                        data: statusData,
                        backgroundColor: [
                            '#ffcdd2', '#bbdefb', '#c8e6c9', '#fff9c4', '#ffcc80', '#f8bbd0', '#c8e6c9'
                        ],
                        borderColor: [
                            '#f44336', '#2196f3', '#4caf50', '#ffeb3b', '#ff9800', '#e91e63', '#4caf50'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
            
            // Criar gráfico de prioridades
            const priorityLabels = ['Normal', 'Importante', 'Urgente'];
            const priorityData = [
                orders.filter(o => o.priority === 'low').length,
                orders.filter(o => o.priority === 'medium').length,
                orders.filter(o => o.priority === 'high').length
            ];
            
            if (priorityChart) {
                priorityChart.destroy();
            }
            
            const priorityCtx = document.getElementById('priority-chart').getContext('2d');
            priorityChart = new Chart(priorityCtx, {
                type: 'doughnut',
                data: {
                    labels: priorityLabels,
                    datasets: [{
                        data: priorityData,
                        backgroundColor: [
                            '#4caf50', '#ff9800', '#f44336'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        }
                    }
                }
            });
        }
        
        // Resetar formulário de OS
        function resetOSForm() {
            document.getElementById('os-form').reset();
            document.getElementById('edit-id').value = '';
            document.getElementById('os-form-title').textContent = 'Nova Ordem de Serviço';
            document.getElementById('save-os-btn').textContent = 'Salvar OS';
            document.getElementById('current-images').innerHTML = '';
            document.getElementById('priority-low').checked = true;
            editingOrderId = null;
        }
        
        // Salvar OS
        function saveOS() {
            const layoutFiles = document.getElementById('layout').files;
            if (layoutFiles.length === 0 && editingOrderId === null) {
                alert('Por favor, selecione pelo menos uma imagem para o layout.');
                return;
            }
            
            // Se estamos editando, manter as imagens existentes
            let existingImages = [];
            if (editingOrderId !== null) {
                const existingOrder = orders.find(o => o.id === editingOrderId);
                if (existingOrder) {
                    existingImages = existingOrder.images || [];
                }
            }
            
            // Processar novas imagens
            const processImages = (callback) => {
                if (layoutFiles.length === 0) {
                    callback(existingImages);
                    return;
                }
                
                const newImages = [];
                let processed = 0;
                
                for (let i = 0; i < layoutFiles.length; i++) {
                    const reader = new FileReader();
                    reader.onload = function(e) {
                        newImages.push(e.target.result);
                        processed++;
                        
                        if (processed === layoutFiles.length) {
                            callback([...existingImages, ...newImages]);
                        }
                    };
                    reader.readAsDataURL(layoutFiles[i]);
                }
            };
            
            processImages((allImages) => {
                const orderData = {
                    id: editingOrderId || (orders.length > 0 ? Math.max(...orders.map(o => o.id)) + 1 : 1),
                    client: document.getElementById('client').value,
                    address: document.getElementById('address').value,
                    whatsapp: document.getElementById('whatsapp').value,
                    service: document.getElementById('service').value,
                    observation: document.getElementById('observation').value,
                    status: document.getElementById('status').value,
                    priority: document.querySelector('input[name="priority"]:checked').value,
                    date: document.getElementById('date').value,
                    images: allImages
                };
                
                if (editingOrderId) {
                    // Atualizar ordem existente
                    const index = orders.findIndex(o => o.id === editingOrderId);
                    if (index !== -1) {
                        orders[index] = orderData;
                    }
                    
                    saveData();
                    showSuccessMessage('Ordem de serviço atualizada com sucesso!');
                    showPage('orders-page');
                    renderOrdersTable();
                } else {
                    // Adicionar nova ordem
                    orders.push(orderData);
                    saveData();
                    showSuccessMessage('Ordem de serviço salva com sucesso!');
                    resetOSForm();
                }
                
                updateDashboard();
                editingOrderId = null;
            });
        }
        
        // Renderizar calendário
        function renderCalendar() {
            const calendarEl = document.getElementById('calendar');
            const monthNames = ['Janeiro', 'Fevereiro', 'Março', 'Abril', 'Maio', 'Junho', 
                               'Julho', 'Agosto', 'Setembro', 'Outubro', 'Novembro', 'Dezembro'];
            
            document.getElementById('current-month').textContent = 
                `${monthNames[currentDate.getMonth()]} ${currentDate.getFullYear()}`;
            
            // Obter primeiro dia do mês e último dia do mês
            const firstDay = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1);
            const lastDay = new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0);
            
            // Dias da semana
            const weekdays = ['Dom', 'Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb'];
            
            // Criar tabela do calendário
            let calendarHtml = `<table class="table table-bordered">
                <thead><tr>`;
            
            // Cabeçalho com dias da semana
            weekdays.forEach(day => {
                calendarHtml += `<th class="text-center">${day}</th>`;
            });
            
            calendarHtml += `</tr></thead><tbody><tr>`;
            
            // Espaços vazios para os dias antes do primeiro dia do mês
            for (let i = 0; i < firstDay.getDay(); i++) {
                calendarHtml += `<td></td>`;
            }
            
            // Dias do mês
            let dayCount = firstDay.getDay();
            for (let day = 1; day <= lastDay.getDate(); day++) {
                if (dayCount % 7 === 0 && dayCount > 0) {
                    calendarHtml += `</tr><tr>`;
                }
                
                const dateStr = `${currentDate.getFullYear()}-${String(currentDate.getMonth() + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                const hasOrders = orders.some(order => order.date === dateStr);
                
                calendarHtml += `<td class="text-center">
                    <div class="calendar-day ${hasOrders ? 'has-orders' : ''}" data-date="${dateStr}">${day}</div>
                </td>`;
                
                dayCount++;
            }
            
            calendarHtml += `</tr></tbody></table>`;
            calendarEl.innerHTML = calendarHtml;
            
            // Adicionar eventos de clique aos dias
            document.querySelectorAll('.calendar-day').forEach(day => {
                day.addEventListener('click', function() {
                    selectedDate = this.getAttribute('data-date');
                    showKanban(selectedDate);
                });
            });
        }
        
        // Mostrar quadro Kanban para uma data específica
        function showKanban(date) {
            showPage('kanban-page');
            
            // Formatar data para exibição
            const [year, month, day] = date.split('-');
            document.getElementById('kanban-date').textContent = `Ordens de Serviço - ${day}/${month}/${year}`;
            
            // Filtrar ordens pela data selecionada
            const dayOrders = orders.filter(order => order.date === date);
            
            // Limpar colunas
            const columns = [
                'aguardando', 'impresso', 'cortado', 'embalado', 
                'emexecucao', 'atrasado', 'concluido'
            ];
            
            columns.forEach(col => {
                document.getElementById(`column-${col}`).innerHTML = '';
            });
            
            // Adicionar cards às colunas
            dayOrders.forEach(order => {
                const statusKey = order.status.toLowerCase().replace(' ', '');
                const column = document.getElementById(`column-${statusKey}`);
                
                if (column) {
                    const priorityClass = `priority-${order.priority}`;
                    const priorityText = order.priority === 'high' ? 'Alta' : order.priority === 'medium' ? 'Média' : 'Baixa';
                    const priorityBadge = order.priority === 'high' ? 'danger' : order.priority === 'medium' ? 'warning' : 'success';
                    
                    // Mostrar a primeira imagem como miniatura, se existir
                    let imageHtml = '';
                    if (order.images && order.images.length > 0) {
                        imageHtml = `
                            <img src="${order.images[0]}" class="image-thumbnail mt-2" 
                                 data-bs-toggle="modal" data-bs-target="#imageModal" 
                                 data-image-src="${order.images[0]}" alt="Imagem do layout">
                        `;
                    }
                    
                    const cardHtml = `
                        <div class="card kanban-card ${priorityClass}" data-id="${order.id}" draggable="true">
                            <div class="card-body">
                                <div class="d-flex justify-content-between">
                                    <h6 class="card-title">${order.client}</h6>
                                    <span class="badge bg-${priorityBadge} order-badge">${priorityText}</span>
                                </div>
                                <p class="card-text small">${order.service}</p>
                                ${imageHtml}
                                <div class="order-actions mt-2">
                                    <a href="https://maps.google.com/?q=${encodeURIComponent(order.address)}" target="_blank" title="Ver no Maps">
                                        <i class="fas fa-map-marker-alt"></i>
                                    </a>
                                    <a href="https://wa.me/${order.whatsapp}" target="_blank" title="Enviar mensagem">
                                        <i class="fab fa-whatsapp"></i>
                                    </a>
                                    <a href="#" class="edit-order" title="Editar" data-id="${order.id}">
                                        <i class="fas fa-edit"></i>
                                    </a>
                                    <a href="#" class="delete-order-kanban" title="Excluir" data-id="${order.id}">
                                        <i class="fas fa-trash"></i>
                                    </a>
                                </div>
                            </div>
                        </div>
                    `;
                    
                    column.innerHTML += cardHtml;
                }
            });
            
            // Adicionar eventos de clique para edição
            document.querySelectorAll('.edit-order').forEach(button => {
                button.addEventListener('click', function(e) {
                    e.preventDefault();
                    const orderId = parseInt(this.getAttribute('data-id'));
                    editOrder(orderId);
                });
            });
            
            // Adicionar eventos de clique para exclusão
            document.querySelectorAll('.delete-order-kanban').forEach(button => {
                button.addEventListener('click', function(e) {
                    e.preventDefault();
                    const orderId = parseInt(this.getAttribute('data-id'));
                    confirmDelete(orderId);
                });
            });
            
            // Configurar arrastar e soltar
            setupDragAndDrop();
        }
        
        // Renderizar tabela de ordens
        function renderOrdersTable() {
            const tableBody = document.getElementById('orders-table-body');
            tableBody.innerHTML = '';
            
            // Aplicar filtros
            const statusFilter = document.getElementById('filter-status').value;
            const priorityFilter = document.getElementById('filter-priority').value;
            const dateFilter = document.getElementById('filter-date').value;
            
            let filteredOrders = orders;
            
            if (statusFilter) {
                filteredOrders = filteredOrders.filter(order => order.status === statusFilter);
            }
            
            if (priorityFilter) {
                filteredOrders = filteredOrders.filter(order => order.priority === priorityFilter);
            }
            
            if (dateFilter) {
                filteredOrders = filteredOrders.filter(order => order.date === dateFilter);
            }
            
            // Ordenar por ID (mais recente primeiro)
            filteredOrders.sort((a, b) => b.id - a.id);
            
            // Preencher tabela
            filteredOrders.forEach(order => {
                const priorityText = order.priority === 'high' ? 'Urgente' : order.priority === 'medium' ? 'Importante' : 'Normal';
                const priorityClass = order.priority === 'high' ? 'danger' : order.priority === 'medium' ? 'warning' : 'success';
                
                const statusClass = order.status === 'Concluído' ? 'success' : 
                                  order.status === 'Atrasado' ? 'danger' : 
                                  order.status === 'Em execução' ? 'warning' : 'secondary';
                
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>#${order.id}</td>
                    <td>${order.client}</td>
                    <td>${order.service}</td>
                    <td>${formatDate(order.date)}</td>
                    <td><span class="badge bg-${statusClass} status-badge">${order.status}</span></td>
                    <td><span class="badge bg-${priorityClass}">${priorityText}</span></td>
                    <td>
                        <a href="#" class="edit-order me-2" data-id="${order.id}">
                            <i class="fas fa-edit"></i>
                        </a>
                        <a href="https://maps.google.com/?q=${encodeURIComponent(order.address)}" target="_blank" class="me-2">
                            <i class="fas fa-map-marker-alt"></i>
                        </a>
                        <a href="https://wa.me/${order.whatsapp}" target="_blank" class="me-2">
                            <i class="fab fa-whatsapp"></i>
                        </a>
                        <a href="#" class="delete-order" data-id="${order.id}">
                            <i class="fas fa-trash"></i>
                        </a>
                    </td>
                `;
                tableBody.appendChild(row);
            });
            
            // Adicionar eventos de clique para edição
            document.querySelectorAll('.edit-order').forEach(button => {
                button.addEventListener('click', function(e) {
                    e.preventDefault();
                    const orderId = parseInt(this.getAttribute('data-id'));
                    editOrder(orderId);
                });
            });
            
            // Adicionar eventos de clique para exclusão
            document.querySelectorAll('.delete-order').forEach(button => {
                button.addEventListener('click', function(e) {
                    e.preventDefault();
                    const orderId = parseInt(this.getAttribute('data-id'));
                    confirmDelete(orderId);
                });
            });
        }
        
        // Formatar data para exibição
        function formatDate(dateStr) {
            const [year, month, day] = dateStr.split('-');
            return `${day}/${month}/${year}`;
        }
        
        // Editar ordem
        function editOrder(orderId) {
            const order = orders.find(o => o.id === orderId);
            if (!order) return;
            
            // Preencher formulário com dados da ordem
            document.getElementById('edit-id').value = order.id;
            document.getElementById('client').value = order.client;
            document.getElementById('address').value = order.address;
            document.getElementById('whatsapp').value = order.whatsapp;
            document.getElementById('service').value = order.service;
            document.getElementById('observation').value = order.observation || '';
            document.getElementById('status').value = order.status;
            document.getElementById('date').value = order.date;
            
            // Selecionar prioridade correta
            document.querySelectorAll('input[name="priority"]').forEach(radio => {
                radio.checked = (radio.value === order.priority);
            });
            
            // Exibir imagens atuais
            const currentImagesContainer = document.getElementById('current-images');
            currentImagesContainer.innerHTML = '';
            
            if (order.images && order.images.length > 0) {
                order.images.forEach((image, index) => {
                    const imageElement = document.createElement('div');
                    imageElement.className = 'gallery-item';
                    imageElement.innerHTML = `
                        <img src="${image}" class="image-thumbnail" 
                             data-bs-toggle="modal" data-bs-target="#imageModal" 
                             data-image-src="${image}" alt="Imagem do layout">
                        <div class="delete-image" data-index="${index}">
                            <i class="fas fa-times"></i>
                        </div>
                    `;
                    currentImagesContainer.appendChild(imageElement);
                });
                
                // Adicionar eventos para excluir imagens
                document.querySelectorAll('.delete-image').forEach(button => {
                    button.addEventListener('click', function() {
                        const index = parseInt(this.getAttribute('data-index'));
                        order.images.splice(index, 1);
                        saveData();
                        editOrder(orderId); // Recarregar o formulário
                    });
                });
            }
            
            // Alterar título do formulário
            document.getElementById('os-form-title').textContent = 'Editar Ordem de Serviço';
            document.getElementById('save-os-btn').textContent = 'Atualizar OS';
            
            // Definir que estamos editando
            editingOrderId = orderId;
            
            // Mostrar página de formulário
            showPage('new-os-page');
        }
        
        // Confirmar exclusão de ordem
        function confirmDelete(orderId) {
            orderToDelete = orderId;
            const deleteModal = new bootstrap.Modal(document.getElementById('deleteModal'));
            deleteModal.show();
        }
        
        // Excluir ordem
        function deleteOrder(orderId) {
            orders = orders.filter(order => order.id !== orderId);
            saveData();
            showSuccessMessage('Ordem de serviço excluída com sucesso!');
            
            // Atualizar visualização atual
            if (document.getElementById('orders-page').classList.contains('d-none') === false) {
                renderOrdersTable();
            } else if (document.getElementById('kanban-page').classList.contains('d-none') === false) {
                showKanban(selectedDate);
            }
            
            updateDashboard();
            orderToDelete = null;
        }
        
        // Configurar arrastar e soltar
        function setupDragAndDrop() {
            const cards = document.querySelectorAll('.kanban-card');
            const columns = document.querySelectorAll('.kanban-column');
            
            cards.forEach(card => {
                card.addEventListener('dragstart', dragStart);
                card.addEventListener('dragend', dragEnd);
            });
            
            columns.forEach(column => {
                column.addEventListener('dragover', dragOver);
                column.addEventListener('dragenter', dragEnter);
                column.addEventListener('dragleave', dragLeave);
                column.addEventListener('drop', dragDrop);
            });
        }
        
        function dragStart() {
            this.classList.add('dragging');
            draggedCard = this;
        }
        
        function dragEnd() {
            this.classList.remove('dragging');
        }
        
        function dragOver(e) {
            e.preventDefault();
        }
        
        function dragEnter(e) {
            e.preventDefault();
            this.style.backgroundColor = '#e3f2fd';
        }
        
        function dragLeave() {
            this.style.backgroundColor = '#f0f8ff';
        }
        
        function dragDrop() {
            this.style.backgroundColor = '#f0f8ff';
            
            if (draggedCard) {
                this.querySelector('.kanban-column-cards').appendChild(draggedCard);
                
                // Atualizar status da ordem no array
                const orderId = parseInt(draggedCard.getAttribute('data-id'));
                const newStatus = this.querySelector('h5').textContent.trim();
                
                const order = orders.find(o => o.id === orderId);
                if (order) {
                    order.status = newStatus;
                    saveData();
                    updateDashboard();
                }
            }
        }
        
        // Renderizar tabela de usuários
        function renderUsersTable() {
            if (currentUser.role !== 'admin') return;
            
            const tableBody = document.getElementById('users-table');
            tableBody.innerHTML = '';
            
            users.forEach(user => {
                // Não permitir que o admin se exclua
                const canDelete = user.username !== 'admin' && user.username !== currentUser.username;
                
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user.username}</td>
                    <td>${user.role === 'admin' ? 'Administrador' : 'Usuário'}</td>
                    <td>
                        ${canDelete ? `<button class="btn btn-sm btn-danger" onclick="deleteUser('${user.username}')">Excluir</button>` : ''}
                    </td>
                `;
                tableBody.appendChild(row);
            });
        }
        
        // Adicionar usuário
        function addUser() {
            if (currentUser.role !== 'admin') return;
            
            const username = document.getElementById('new-username').value;
            const password = document.getElementById('new-password').value;
            const role = document.getElementById('user-role').value;
            
            // Verificar se o usuário já existe
            if (users.some(u => u.username === username)) {
                alert('Já existe um usuário com este nome.');
                return;
            }
            
            // Adicionar usuário
            users.push({ username, password, role });
            saveData();
            
            // Limpar formulário e atualizar tabela
            document.getElementById('user-form').reset();
            renderUsersTable();
            
            alert('Usuário adicionado com sucesso!');
        }
        
        // Excluir usuário
        function deleteUser(username) {
            if (currentUser.role !== 'admin') return;
            
            if (confirm(`Tem certeza que deseja excluir o usuário "${username}"?`)) {
                users = users.filter(u => u.username !== username);
                saveData();
                renderUsersTable();
            }
        }
        
        // Inicializar a aplicação quando o documento estiver carregado
        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
