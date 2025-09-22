# Finance
[index_mejorado.html](https://github.com/user-attachments/files/22480750/index_mejorado.html)
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control de Finanzas</title>
    <link rel="stylesheet" href="style.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <div class="app">
        <!-- Header -->
        <header class="header">
            <h1 id="page-title">Control de Finanzas</h1>
            <div id="greeting" class="greeting">Hola, Usuario</div>
        </header>

        <!-- Main Content -->
        <main class="main-content">
            <!-- Dashboard Screen -->
            <div id="dashboard" class="screen active">
                <div class="metrics-grid">
                    <div class="metric-card income-current">
                        <div class="metric-label">Ingresos mes actual</div>
                        <div class="metric-value">$4.575.000</div>
                    </div>
                    <div class="metric-card income-previous">
                        <div class="metric-label">Ingresos mes anterior</div>
                        <div class="metric-value">$3.820.000</div>
                    </div>
                    <div class="metric-card pending">
                        <div class="metric-label">Facturas pendientes</div>
                        <div class="metric-value">12</div>
                    </div>
                    <div class="metric-card paid">
                        <div class="metric-label">Facturas pagadas</div>
                        <div class="metric-value">28</div>
                    </div>
                    <div class="metric-card overdue">
                        <div class="metric-label">Facturas vencidas</div>
                        <div class="metric-value">3</div>
                    </div>
                </div>

                <div class="chart-container">
                    <canvas id="incomeChart"></canvas>
                </div>

                <div class="quick-actions">
                    <h3>Acceso Rápido</h3>
                    <div class="quick-actions-grid">
                        <button class="quick-action-btn" onclick="app.switchTab('clientes')">
                            <span class="icon">👥</span>
                            Ver Clientes
                        </button>
                        <button class="quick-action-btn" onclick="app.showModal('newInvoiceModal')">
                            <span class="icon">📄</span>
                            Nueva Factura
                        </button>
                        <button class="quick-action-btn" onclick="app.switchTab('servicios')">
                            <span class="icon">⚙️</span>
                            Servicios
                        </button>
                        <button class="quick-action-btn" onclick="app.switchTab('reportes')">
                            <span class="icon">📊</span>
                            Reportes
                        </button>
                    </div>
                </div>
            </div>

            <!-- Clients Screen -->
            <div id="clientes" class="screen">
                <div class="screen-header">
                    <h2>Mis Clientes</h2>
                    <div class="client-count">Total: <span id="client-count">0</span> clientes</div>
                </div>

                <div class="search-container">
                    <input type="text" id="client-search" placeholder="Buscar cliente..." class="search-input">
                </div>

                <div id="client-list" class="client-list">
                    <!-- Clients will be rendered here -->
                </div>

                <button class="fab" onclick="app.showModal('newClientModal')">+</button>
            </div>

            <!-- Invoices Screen -->
            <div id="facturas" class="screen">
                <div class="screen-header">
                    <h2>Gestión de Facturas</h2>
                </div>

                <div class="filter-tabs">
                    <button class="filter-tab active" data-filter="todas" onclick="app.filterInvoices('todas')">
                        Todas <span class="badge" id="all-count">0</span>
                    </button>
                    <button class="filter-tab" data-filter="Pagada" onclick="app.filterInvoices('Pagada')">
                        Pagadas <span class="badge success" id="paid-count">0</span>
                    </button>
                    <button class="filter-tab" data-filter="Pendiente" onclick="app.filterInvoices('Pendiente')">
                        Pendientes <span class="badge warning" id="pending-count">0</span>
                    </button>
                    <button class="filter-tab" data-filter="Vencida" onclick="app.filterInvoices('Vencida')">
                        Vencidas <span class="badge error" id="overdue-count">0</span>
                    </button>
                </div>

                <div id="invoice-list" class="invoice-list">
                    <!-- Invoices will be rendered here -->
                </div>

                <button class="fab" onclick="app.showModal('newInvoiceModal')">+</button>
            </div>

            <!-- Services Screen -->
            <div id="servicios" class="screen">
                <div class="screen-header">
                    <h2>Catálogo de Servicios</h2>
                    <div class="service-count">Total: <span id="service-count">0</span> servicios activos</div>
                </div>

                <div id="service-list" class="service-list">
                    <!-- Services will be rendered here -->
                </div>

                <button class="fab" onclick="app.showModal('newServiceModal')">+</button>
            </div>

            <!-- Reports Screen -->
            <div id="reportes" class="screen">
                <div class="screen-header">
                    <h2>Reportes y Análisis</h2>
                </div>

                <!-- Export Section -->
                <div class="export-section">
                    <h3>Exportar Informes</h3>
                    <div class="export-buttons">
                        <button class="btn-primary export-btn" onclick="app.exportToExcel()">
                            📊 Exportar para Contador (CSV)
                        </button>
                        <button class="btn-secondary export-btn" onclick="app.exportData()">
                            💾 Backup Completo (JSON)
                        </button>
                    </div>
                </div>

                <!-- Monthly Sales Chart -->
                <div class="report-section">
                    <h3>Evolución de Ventas Mensuales</h3>
                    <div class="chart-container">
                        <canvas id="monthlyChart"></canvas>
                    </div>
                </div>

                <!-- Monthly Sales Table -->
                <div class="report-section">
                    <h3>Detalle Mensual</h3>
                    <div class="table-container">
                        <table class="report-table">
                            <thead>
                                <tr>
                                    <th>Mes</th>
                                    <th>Ingresos</th>
                                    <th>Facturas</th>
                                    <th>Clientes</th>
                                    <th>Promedio/Factura</th>
                                </tr>
                            </thead>
                            <tbody id="monthly-sales-table">
                                <!-- Monthly data will be rendered here -->
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Top Services -->
                <div class="report-section">
                    <h3>Servicios Más Rentables</h3>
                    <div id="top-services" class="top-items-container">
                        <!-- Top services will be rendered here -->
                    </div>
                </div>

                <!-- Client Analysis -->
                <div class="report-section">
                    <h3>Mejores Clientes</h3>
                    <div id="client-analysis" class="top-items-container">
                        <!-- Client analysis will be rendered here -->
                    </div>
                </div>
            </div>

            <!-- Config Screen -->
            <div id="config" class="screen">
                <div class="screen-header">
                    <h2>Configuración</h2>
                </div>

                <div class="config-sections">
                    <div class="config-section">
                        <h3>Perfil de la Empresa</h3>
                        <div class="config-item">
                            <label>Nombre de la empresa</label>
                            <input type="text" value="Mi Empresa SRL" readonly>
                        </div>
                        <div class="config-item">
                            <label>CUIT</label>
                            <input type="text" value="20-12345678-9" readonly>
                        </div>
                        <div class="config-item">
                            <label>Dirección</label>
                            <input type="text" value="Av. Corrientes 1234, CABA" readonly>
                        </div>
                        <div class="config-item">
                            <label>Email</label>
                            <input type="email" value="contacto@miempresa.com" readonly>
                        </div>
                    </div>

                    <div class="config-section">
                        <h3>Configuración de Facturación</h3>
                        <div class="config-item">
                            <label>Moneda</label>
                            <select>
                                <option value="ARS" selected>Peso Argentino ($)</option>
                                <option value="USD">Dólar Estadounidense (US$)</option>
                            </select>
                        </div>
                        <div class="config-item">
                            <label>IVA por defecto</label>
                            <select>
                                <option value="21" selected>21%</option>
                                <option value="10.5">10,5%</option>
                                <option value="0">0% (Exento)</option>
                            </select>
                        </div>
                    </div>

                    <div class="config-section">
                        <h3>Datos y Backup</h3>
                        <button class="config-btn" onclick="app.exportData()">Exportar Datos</button>
                        <button class="config-btn" onclick="app.exportToExcel()">Informe para Contador</button>
                        <button class="config-btn warning" onclick="app.clearData()">Limpiar Datos</button>
                    </div>
                </div>
            </div>
        </main>

        <!-- Bottom Navigation -->
        <nav class="bottom-nav">
            <button class="nav-item active" data-tab="dashboard" onclick="app.switchTab('dashboard')">
                <span class="nav-icon">📊</span>
                <span class="nav-label">Dashboard</span>
            </button>
            <button class="nav-item" data-tab="clientes" onclick="app.switchTab('clientes')">
                <span class="nav-icon">👥</span>
                <span class="nav-label">Clientes</span>
            </button>
            <button class="nav-item" data-tab="facturas" onclick="app.switchTab('facturas')">
                <span class="nav-icon">📄</span>
                <span class="nav-label">Facturas</span>
            </button>
            <button class="nav-item" data-tab="servicios" onclick="app.switchTab('servicios')">
                <span class="nav-icon">⚙️</span>
                <span class="nav-label">Servicios</span>
            </button>
            <button class="nav-item" data-tab="reportes" onclick="app.switchTab('reportes')">
                <span class="nav-icon">📈</span>
                <span class="nav-label">Reportes</span>
            </button>
            <button class="nav-item" data-tab="config" onclick="app.switchTab('config')">
                <span class="nav-icon">⚙️</span>
                <span class="nav-label">Config</span>
            </button>
        </nav>

        <!-- Modals -->
        <!-- New Client Modal -->
        <div id="newClientModal" class="modal">
            <div class="modal-content">
                <div class="modal-header">
                    <h3>Nuevo Cliente</h3>
                    <button class="modal-close" onclick="app.hideModal('newClientModal')">&times;</button>
                </div>
                <form id="newClientForm" class="modal-form">
                    <div class="form-group">
                        <label>Nombre</label>
                        <input type="text" name="nombre" required>
                    </div>
                    <div class="form-group">
                        <label>Teléfono</label>
                        <input type="tel" name="telefono" placeholder="+54 11 1234-5678" required>
                    </div>
                    <div class="form-group">
                        <label>Email</label>
                        <input type="email" name="email" required>
                    </div>
                    <div class="form-group">
                        <label>Dirección</label>
                        <textarea name="direccion" rows="2" placeholder="Dirección completa"></textarea>
                    </div>
                    <div class="modal-actions">
                        <button type="button" class="btn-secondary" onclick="app.hideModal('newClientModal')">Cancelar</button>
                        <button type="submit" class="btn-primary">Guardar Cliente</button>
                    </div>
                </form>
            </div>
        </div>

        <!-- Edit Client Modal -->
        <div id="editClientModal" class="modal">
            <div class="modal-content">
                <div class="modal-header">
                    <h3>Editar Cliente</h3>
                    <button class="modal-close" onclick="app.hideModal('editClientModal')">&times;</button>
                </div>
                <form id="editClientForm" class="modal-form">
                    <div class="form-group">
                        <label>Nombre</label>
                        <input type="text" name="nombre" required>
                    </div>
                    <div class="form-group">
                        <label>Teléfono</label>
                        <input type="tel" name="telefono" required>
                    </div>
                    <div class="form-group">
                        <label>Email</label>
                        <input type="email" name="email" required>
                    </div>
                    <div class="form-group">
                        <label>Dirección</label>
                        <textarea name="direccion" rows="2"></textarea>
                    </div>
                    <div class="modal-actions">
                        <button type="button" class="btn-secondary" onclick="app.hideModal('editClientModal')">Cancelar</button>
                        <button type="submit" class="btn-primary">Actualizar Cliente</button>
                    </div>
                </form>
            </div>
        </div>

        <!-- New Invoice Modal -->
        <div id="newInvoiceModal" class="modal">
            <div class="modal-content large">
                <div class="modal-header">
                    <h3>Nueva Factura</h3>
                    <button class="modal-close" onclick="app.hideModal('newInvoiceModal')">&times;</button>
                </div>
                <form id="newInvoiceForm" class="modal-form">
                    <div class="form-row">
                        <div class="form-group">
                            <label>Cliente</label>
                            <select name="cliente" required>
                                <option value="">Seleccionar cliente</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>Fecha de Emisión</label>
                            <input type="date" name="fecha" required>
                        </div>
                    </div>
                    <div class="form-row">
                        <div class="form-group">
                            <label>Fecha de Vencimiento</label>
                            <input type="date" name="vencimiento" required>
                        </div>
                        <div class="form-group">
                            <label>Estado</label>
                            <select name="estado">
                                <option value="Pendiente" selected>Pendiente</option>
                                <option value="Pagada">Pagada</option>
                            </select>
                        </div>
                    </div>

                    <h4>Servicios</h4>
                    <div id="invoice-services" class="invoice-services">
                        <div class="service-row">
                            <select name="servicios[0][nombre]" class="service-select">
                                <option value="">Seleccionar servicio</option>
                            </select>
                            <input type="number" name="servicios[0][cantidad]" placeholder="Cantidad" min="1" value="1">
                            <input type="number" name="servicios[0][precio]" placeholder="Precio" step="0.01" min="0">
                            <button type="button" class="btn-remove" onclick="app.removeServiceRow(this)">-</button>
                        </div>
                    </div>
                    <button type="button" class="btn-add" onclick="app.addServiceRow()">+ Agregar Servicio</button>

                    <div class="form-group">
                        <label>Notas</label>
                        <textarea name="notas" rows="3" placeholder="Notas adicionales..."></textarea>
                    </div>

                    <div class="invoice-total">
                        <div class="total-row">
                            <span>Subtotal:</span>
                            <span id="invoice-subtotal">$0</span>
                        </div>
                        <div class="total-row">
                            <span>IVA (21%):</span>
                            <span id="invoice-tax">$0</span>
                        </div>
                        <div class="total-row total">
                            <span>Total:</span>
                            <span id="invoice-total">$0</span>
                        </div>
                    </div>

                    <div class="modal-actions">
                        <button type="button" class="btn-secondary" onclick="app.hideModal('newInvoiceModal')">Cancelar</button>
                        <button type="submit" class="btn-primary">Crear Factura</button>
                    </div>
                </form>
            </div>
        </div>

        <!-- Edit Invoice Modal -->
        <div id="editInvoiceModal" class="modal">
            <div class="modal-content large">
                <div class="modal-header">
                    <h3>Editar Factura</h3>
                    <button class="modal-close" onclick="app.hideModal('editInvoiceModal')">&times;</button>
                </div>
                <form id="editInvoiceForm" class="modal-form">
                    <div class="form-row">
                        <div class="form-group">
                            <label>Cliente</label>
                            <select name="cliente" required>
                                <option value="">Seleccionar cliente</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>Fecha de Emisión</label>
                            <input type="date" name="fecha" required>
                        </div>
                    </div>
                    <div class="form-row">
                        <div class="form-group">
                            <label>Fecha de Vencimiento</label>
                            <input type="date" name="vencimiento" required>
                        </div>
                        <div class="form-group">
                            <label>Estado</label>
                            <select name="estado">
                                <option value="Pendiente">Pendiente</option>
                                <option value="Pagada">Pagada</option>
                                <option value="Vencida">Vencida</option>
                            </select>
                        </div>
                    </div>

                    <h4>Servicios</h4>
                    <div id="edit-invoice-services" class="invoice-services">
                        <!-- Services will be populated here -->
                    </div>
                    <button type="button" class="btn-add" onclick="app.addServiceRowToEditForm()">+ Agregar Servicio</button>

                    <div class="form-group">
                        <label>Notas</label>
                        <textarea name="notas" rows="3" placeholder="Notas adicionales..."></textarea>
                    </div>

                    <div class="invoice-total">
                        <div class="total-row">
                            <span>Subtotal:</span>
                            <span id="edit-invoice-subtotal">$0</span>
                        </div>
                        <div class="total-row">
                            <span>IVA (21%):</span>
                            <span id="edit-invoice-tax">$0</span>
                        </div>
                        <div class="total-row total">
                            <span>Total:</span>
                            <span id="edit-invoice-total">$0</span>
                        </div>
                    </div>

                    <div class="modal-actions">
                        <button type="button" class="btn-secondary" onclick="app.hideModal('editInvoiceModal')">Cancelar</button>
                        <button type="submit" class="btn-primary">Actualizar Factura</button>
                    </div>
                </form>
            </div>
        </div>

        <!-- New Service Modal -->
        <div id="newServiceModal" class="modal">
            <div class="modal-content">
                <div class="modal-header">
                    <h3>Nuevo Servicio</h3>
                    <button class="modal-close" onclick="app.hideModal('newServiceModal')">&times;</button>
                </div>
                <form id="newServiceForm" class="modal-form">
                    <div class="form-group">
                        <label>Nombre del servicio</label>
                        <input type="text" name="nombre" required>
                    </div>
                    <div class="form-row">
                        <div class="form-group">
                            <label>Precio (ARS)</label>
                            <input type="number" name="precio" step="0.01" min="0" required placeholder="0.00">
                        </div>
                        <div class="form-group">
                            <label>Unidad</label>
                            <select name="unidad">
                                <option value="hora">Por hora</option>
                                <option value="proyecto">Por proyecto</option>
                                <option value="mes">Por mes</option>
                                <option value="unidad">Por unidad</option>
                            </select>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Descripción</label>
                        <textarea name="descripcion" rows="3" placeholder="Describe el servicio..."></textarea>
                    </div>
                    <div class="modal-actions">
                        <button type="button" class="btn-secondary" onclick="app.hideModal('newServiceModal')">Cancelar</button>
                        <button type="submit" class="btn-primary">Guardar Servicio</button>
                    </div>
                </form>
            </div>
        </div>

        <!-- Edit Service Modal -->
        <div id="editServiceModal" class="modal">
            <div class="modal-content">
                <div class="modal-header">
                    <h3>Editar Servicio</h3>
                    <button class="modal-close" onclick="app.hideModal('editServiceModal')">&times;</button>
                </div>
                <form id="editServiceForm" class="modal-form">
                    <div class="form-group">
                        <label>Nombre del servicio</label>
                        <input type="text" name="nombre" required>
                    </div>
                    <div class="form-row">
                        <div class="form-group">
                            <label>Precio (ARS)</label>
                            <input type="number" name="precio" step="0.01" min="0" required>
                        </div>
                        <div class="form-group">
                            <label>Unidad</label>
                            <select name="unidad">
                                <option value="hora">Por hora</option>
                                <option value="proyecto">Por proyecto</option>
                                <option value="mes">Por mes</option>
                                <option value="unidad">Por unidad</option>
                            </select>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Descripción</label>
                        <textarea name="descripcion" rows="3"></textarea>
                    </div>
                    <div class="modal-actions">
                        <button type="button" class="btn-secondary" onclick="app.hideModal('editServiceModal')">Cancelar</button>
                        <button type="submit" class="btn-primary">Actualizar Servicio</button>
                    </div>
                </form>
            </div>
        </div>

        <!-- Client Profile Modal -->
        <div id="clientProfileModal" class="modal">
            <div class="modal-content">
                <div class="modal-header">
                    <h3 id="client-profile-name">Perfil del Cliente</h3>
                    <button class="modal-close" onclick="app.hideModal('clientProfileModal')">&times;</button>
                </div>
                <div class="client-profile-content">
                    <div class="client-info">
                        <div class="info-row">
                            <strong>Teléfono:</strong>
                            <span id="client-profile-phone"></span>
                        </div>
                        <div class="info-row">
                            <strong>Email:</strong>
                            <span id="client-profile-email"></span>
                        </div>
                        <div class="info-row">
                            <strong>Dirección:</strong>
                            <span id="client-profile-address"></span>
                        </div>
                        <div class="info-row">
                            <strong>Total Facturado:</strong>
                            <span id="client-profile-total" class="amount"></span>
                        </div>
                        <div class="info-row">
                            <strong>Facturas:</strong>
                            <span id="client-profile-invoices"></span>
                        </div>
                    </div>
                    <div class="client-actions">
                        <button class="btn-secondary" onclick="app.editClient()">Editar Cliente</button>
                        <button class="btn-danger" onclick="app.deleteClient()">Eliminar Cliente</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Invoice Detail Modal -->
        <div id="invoiceDetailModal" class="modal">
            <div class="modal-content large">
                <div class="modal-header">
                    <h3 id="invoice-detail-title">Detalle de Factura</h3>
                    <button class="modal-close" onclick="app.hideModal('invoiceDetailModal')">&times;</button>
                </div>
                <div class="invoice-detail-content">
                    <div class="invoice-header">
                        <div class="invoice-info">
                            <div class="info-row">
                                <strong>Número:</strong>
                                <span id="invoice-detail-number"></span>
                            </div>
                            <div class="info-row">
                                <strong>Cliente:</strong>
                                <span id="invoice-detail-client"></span>
                            </div>
                            <div class="info-row">
                                <strong>Fecha:</strong>
                                <span id="invoice-detail-date"></span>
                            </div>
                            <div class="info-row">
                                <strong>Vencimiento:</strong>
                                <span id="invoice-detail-due"></span>
                            </div>
                            <div class="info-row">
                                <strong>Estado:</strong>
                                <span id="invoice-detail-status" class="status-badge"></span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="invoice-services-detail">
                        <h4>Servicios</h4>
                        <div id="invoice-detail-services"></div>
                    </div>

                    <div class="invoice-totals">
                        <div class="total-row">
                            <span>Subtotal:</span>
                            <span id="invoice-detail-subtotal"></span>
                        </div>
                        <div class="total-row">
                            <span>IVA (21%):</span>
                            <span id="invoice-detail-tax"></span>
                        </div>
                        <div class="total-row total">
                            <span>Total:</span>
                            <span id="invoice-detail-total"></span>
                        </div>
                    </div>

                    <div class="invoice-notes" id="invoice-detail-notes-section">
                        <h4>Notas</h4>
                        <p id="invoice-detail-notes"></p>
                    </div>

                    <div class="invoice-actions">
                        <button class="btn-success" onclick="app.markAsPaid()" id="mark-paid-btn">Marcar como Pagada</button>
                        <button class="btn-primary" onclick="app.shareInvoice()">Compartir PDF</button>
                        <button class="btn-secondary" onclick="app.editInvoice()">Editar</button>
                        <button class="btn-danger" onclick="app.deleteInvoice()">Eliminar</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Service Detail Modal -->
        <div id="serviceDetailModal" class="modal">
            <div class="modal-content">
                <div class="modal-header">
                    <h3 id="service-detail-name">Detalle del Servicio</h3>
                    <button class="modal-close" onclick="app.hideModal('serviceDetailModal')">&times;</button>
                </div>
                <div class="service-detail-content">
                    <div class="service-info">
                        <div class="info-row">
                            <strong>Precio:</strong>
                            <span id="service-detail-price" class="amount"></span>
                        </div>
                        <div class="info-row">
                            <strong>Unidad:</strong>
                            <span id="service-detail-unit"></span>
                        </div>
                        <div class="info-row">
                            <strong>Descripción:</strong>
                            <span id="service-detail-description"></span>
                        </div>
                    </div>
                    
                    <div class="service-stats">
                        <h4>Estadísticas</h4>
                        <div class="stats-grid">
                            <div class="stat-card">
                                <div class="stat-value" id="service-detail-sold"></div>
                                <div class="stat-label">Veces vendido</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-value" id="service-detail-revenue"></div>
                                <div class="stat-label">Ingresos generados</div>
                            </div>
                        </div>
                    </div>

                    <div class="service-actions">
                        <button class="btn-secondary" onclick="app.editService()">Editar Servicio</button>
                        <button class="btn-danger" onclick="app.deleteService()">Eliminar Servicio</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script src="app_mejorado.js"></script>
</body>
</html>
[style_mejorado.css](https://github.com/user-attachments/files/22480751/style_mejorado.css)

/* Variables CSS - Actualizado para Argentina */
:root {
    --primary-color: #2196F3;
    --secondary-color: #f5f5f5;
    --success-color: #4CAF50;
    --warning-color: #FF9800;
    --error-color: #F44336;
    --text-primary: #212121;
    --text-secondary: #757575;
    --background: #fafafa;
    --white: #ffffff;
    --shadow: 0 2px 4px rgba(0,0,0,0.1);
    --shadow-lg: 0 4px 12px rgba(0,0,0,0.15);
    --border-radius: 8px;
    --spacing-xs: 4px;
    --spacing-sm: 8px;
    --spacing-md: 16px;
    --spacing-lg: 24px;
    --spacing-xl: 32px;

    /* Colores específicos para Argentina */
    --argentina-blue: #74acdf;
    --argentina-yellow: #f7c52d;
}

/* Reset y Base */
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background-color: var(--background);
    color: var(--text-primary);
    line-height: 1.6;
}

.app {
    max-width: 100vw;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

/* Header */
.header {
    background: linear-gradient(135deg, var(--primary-color) 0%, var(--argentina-blue) 100%);
    color: var(--white);
    padding: var(--spacing-md) var(--spacing-md) var(--spacing-lg);
    text-align: center;
    box-shadow: var(--shadow);
}

.header h1 {
    font-size: 1.5rem;
    font-weight: 600;
    margin-bottom: var(--spacing-xs);
}

.greeting {
    font-size: 0.9rem;
    opacity: 0.9;
}

/* Main Content */
.main-content {
    flex: 1;
    padding: var(--spacing-md);
    padding-bottom: 80px; /* Space for bottom nav */
}

.screen {
    display: none;
    animation: fadeIn 0.3s ease-in-out;
}

.screen.active {
    display: block;
}

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
}

/* Screen Headers */
.screen-header {
    margin-bottom: var(--spacing-lg);
    text-align: center;
}

.screen-header h2 {
    font-size: 1.4rem;
    color: var(--text-primary);
    margin-bottom: var(--spacing-xs);
}

.client-count, .service-count {
    color: var(--text-secondary);
    font-size: 0.9rem;
}

/* Dashboard */
.metrics-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: var(--spacing-md);
    margin-bottom: var(--spacing-lg);
}

.metric-card {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    text-align: center;
    border-left: 4px solid var(--primary-color);
    transition: transform 0.2s;
}

.metric-card:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
}

.metric-card.pending { border-left-color: var(--warning-color); }
.metric-card.paid { border-left-color: var(--success-color); }
.metric-card.overdue { border-left-color: var(--error-color); }

.metric-label {
    font-size: 0.85rem;
    color: var(--text-secondary);
    margin-bottom: var(--spacing-xs);
}

.metric-value {
    font-size: 1.5rem;
    font-weight: 600;
    color: var(--text-primary);
}

.chart-container {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    margin-bottom: var(--spacing-lg);
    height: 300px;
}

.quick-actions {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
}

.quick-actions h3 {
    margin-bottom: var(--spacing-md);
    font-size: 1.1rem;
}

.quick-actions-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    gap: var(--spacing-md);
}

.quick-action-btn {
    background: var(--secondary-color);
    border: none;
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    cursor: pointer;
    text-align: center;
    transition: all 0.2s;
}

.quick-action-btn:hover {
    background: var(--primary-color);
    color: var(--white);
    transform: translateY(-2px);
}

.quick-action-btn .icon {
    display: block;
    font-size: 1.5rem;
    margin-bottom: var(--spacing-xs);
}

/* Reports Section */
.export-section {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    margin-bottom: var(--spacing-lg);
}

.export-section h3 {
    margin-bottom: var(--spacing-md);
    color: var(--text-primary);
}

.export-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: var(--spacing-md);
}

.export-btn {
    flex: 1;
    min-width: 200px;
    padding: var(--spacing-md);
    border: none;
    border-radius: var(--border-radius);
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s;
}

.export-btn:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
}

.report-section {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    margin-bottom: var(--spacing-lg);
}

.report-section h3 {
    margin-bottom: var(--spacing-md);
    color: var(--text-primary);
    border-bottom: 2px solid var(--secondary-color);
    padding-bottom: var(--spacing-sm);
}

.table-container {
    overflow-x: auto;
}

.report-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.9rem;
}

.report-table th,
.report-table td {
    padding: var(--spacing-sm) var(--spacing-md);
    text-align: left;
    border-bottom: 1px solid var(--secondary-color);
}

.report-table th {
    background: var(--secondary-color);
    font-weight: 600;
    color: var(--text-primary);
}

.report-table tr:hover {
    background: rgba(33, 150, 243, 0.05);
}

.report-table td:nth-child(2),
.report-table td:nth-child(5) {
    font-weight: 600;
    color: var(--primary-color);
}

.top-items-container {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-md);
}

.top-service-item,
.top-client-item {
    display: flex;
    align-items: center;
    padding: var(--spacing-md);
    background: var(--secondary-color);
    border-radius: var(--border-radius);
    transition: all 0.2s;
}

.top-service-item:hover,
.top-client-item:hover {
    transform: translateX(4px);
    box-shadow: var(--shadow);
}

.service-rank,
.client-rank {
    background: var(--primary-color);
    color: var(--white);
    width: 32px;
    height: 32px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 600;
    font-size: 0.9rem;
    margin-right: var(--spacing-md);
}

.service-info,
.client-info {
    flex: 1;
}

.service-name,
.client-name {
    font-weight: 600;
    margin-bottom: var(--spacing-xs);
}

.service-stats,
.client-stats {
    display: flex;
    gap: var(--spacing-md);
    font-size: 0.85rem;
    color: var(--text-secondary);
}

/* Search */
.search-container {
    margin-bottom: var(--spacing-lg);
}

.search-input {
    width: 100%;
    padding: var(--spacing-md);
    border: 1px solid #ddd;
    border-radius: var(--border-radius);
    font-size: 1rem;
    transition: border-color 0.2s;
}

.search-input:focus {
    outline: none;
    border-color: var(--primary-color);
}

/* Client List */
.client-list {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-md);
}

.client-item {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    cursor: pointer;
    transition: all 0.2s;
    border-left: 4px solid var(--primary-color);
}

.client-item:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
}

.client-name {
    font-weight: 600;
    margin-bottom: var(--spacing-xs);
    color: var(--text-primary);
}

.client-info {
    font-size: 0.9rem;
    color: var(--text-secondary);
    margin-bottom: var(--spacing-sm);
}

.client-actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.client-invoices {
    font-size: 0.8rem;
    color: var(--text-secondary);
    background: var(--secondary-color);
    padding: 2px 8px;
    border-radius: 12px;
}

.view-profile-btn {
    background: var(--primary-color);
    color: var(--white);
    border: none;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: 0.9rem;
    transition: all 0.2s;
}

.view-profile-btn:hover {
    background: var(--argentina-blue);
    transform: scale(1.05);
}

/* Filter Tabs */
.filter-tabs {
    display: flex;
    background: var(--white);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    margin-bottom: var(--spacing-lg);
    overflow-x: auto;
}

.filter-tab {
    flex: 1;
    background: transparent;
    border: none;
    padding: var(--spacing-md);
    cursor: pointer;
    font-size: 0.9rem;
    transition: all 0.2s;
    white-space: nowrap;
    border-bottom: 3px solid transparent;
}

.filter-tab:hover {
    background: rgba(33, 150, 243, 0.1);
}

.filter-tab.active {
    background: var(--primary-color);
    color: var(--white);
    border-bottom-color: var(--argentina-blue);
}

.badge {
    background: var(--text-secondary);
    color: var(--white);
    padding: 2px 6px;
    border-radius: 10px;
    font-size: 0.7rem;
    margin-left: var(--spacing-xs);
}

.badge.success { background: var(--success-color); }
.badge.warning { background: var(--warning-color); }
.badge.error { background: var(--error-color); }

/* Invoice List */
.invoice-list {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-md);
}

.invoice-item {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    cursor: pointer;
    transition: all 0.2s;
    border-left: 4px solid var(--primary-color);
}

.invoice-item:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
}

.invoice-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: var(--spacing-sm);
}

.invoice-number {
    font-weight: 600;
    color: var(--text-primary);
}

.invoice-amount {
    font-weight: 600;
    font-size: 1.1rem;
    color: var(--primary-color);
}

.invoice-client {
    color: var(--text-secondary);
    margin-bottom: var(--spacing-xs);
    font-size: 0.95rem;
}

.invoice-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.9rem;
}

.invoice-date {
    color: var(--text-secondary);
}

.status-badge {
    padding: 4px 8px;
    border-radius: 12px;
    font-size: 0.8rem;
    font-weight: 500;
}

.status-badge.Pagada {
    background: var(--success-color);
    color: var(--white);
}

.status-badge.Pendiente {
    background: var(--warning-color);
    color: var(--white);
}

.status-badge.Vencida {
    background: var(--error-color);
    color: var(--white);
}

/* Service List */
.service-list {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-md);
}

.service-item {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    cursor: pointer;
    transition: all 0.2s;
    border-left: 4px solid var(--success-color);
}

.service-item:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
}

.service-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: var(--spacing-sm);
}

.service-name {
    font-weight: 600;
    color: var(--text-primary);
}

.service-price {
    font-weight: 600;
    color: var(--primary-color);
    font-size: 1.1rem;
}

.service-description {
    color: var(--text-secondary);
    margin-bottom: var(--spacing-sm);
    font-size: 0.9rem;
}

.service-stats {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--spacing-md);
    font-size: 0.8rem;
    color: var(--text-secondary);
}

/* Config Screen */
.config-sections {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-lg);
}

.config-section {
    background: var(--white);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
}

.config-section h3 {
    margin-bottom: var(--spacing-md);
    font-size: 1.1rem;
    color: var(--text-primary);
    border-bottom: 2px solid var(--secondary-color);
    padding-bottom: var(--spacing-sm);
}

.config-item {
    margin-bottom: var(--spacing-md);
}

.config-item:last-child {
    margin-bottom: 0;
}

.config-item label {
    display: block;
    margin-bottom: var(--spacing-xs);
    font-weight: 500;
    color: var(--text-primary);
}

.config-item input,
.config-item select {
    width: 100%;
    padding: var(--spacing-sm);
    border: 1px solid #ddd;
    border-radius: var(--border-radius);
    font-size: 1rem;
    transition: border-color 0.2s;
}

.config-item input:focus,
.config-item select:focus {
    outline: none;
    border-color: var(--primary-color);
}

.config-btn {
    background: var(--primary-color);
    color: var(--white);
    border: none;
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    cursor: pointer;
    margin-right: var(--spacing-sm);
    margin-bottom: var(--spacing-sm);
    transition: all 0.2s;
}

.config-btn:hover {
    transform: translateY(-1px);
    box-shadow: var(--shadow);
}

.config-btn.warning {
    background: var(--error-color);
}

/* Floating Action Button */
.fab {
    position: fixed;
    bottom: 90px;
    right: var(--spacing-md);
    width: 56px;
    height: 56px;
    background: var(--primary-color);
    color: var(--white);
    border: none;
    border-radius: 50%;
    font-size: 1.5rem;
    cursor: pointer;
    box-shadow: var(--shadow-lg);
    transition: all 0.2s;
}

.fab:hover {
    transform: scale(1.1);
    background: var(--argentina-blue);
}

/* Bottom Navigation */
.bottom-nav {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background: var(--white);
    display: flex;
    border-top: 1px solid #eee;
    box-shadow: 0 -2px 8px rgba(0,0,0,0.1);
    z-index: 1000;
}

.nav-item {
    flex: 1;
    background: transparent;
    border: none;
    padding: var(--spacing-sm);
    cursor: pointer;
    text-align: center;
    transition: all 0.2s;
    color: var(--text-secondary);
}

.nav-item:hover {
    background: rgba(33, 150, 243, 0.1);
}

.nav-item.active {
    color: var(--primary-color);
    background: rgba(33, 150, 243, 0.1);
}

.nav-icon {
    display: block;
    font-size: 1.2rem;
    margin-bottom: 4px;
}

.nav-label {
    font-size: 0.7rem;
    display: block;
}

/* Modals */
.modal {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.5);
    z-index: 1000;
    padding: var(--spacing-md);
}

.modal.active {
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadeIn 0.3s;
}

.modal-content {
    background: var(--white);
    border-radius: var(--border-radius);
    box-shadow: var(--shadow-lg);
    max-width: 90vw;
    max-height: 90vh;
    overflow-y: auto;
    width: 100%;
    max-width: 500px;
}

.modal-content.large {
    max-width: 95vw;
    max-width: 700px;
}

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: var(--spacing-md);
    border-bottom: 1px solid #eee;
    background: var(--secondary-color);
}

.modal-header h3 {
    margin: 0;
    color: var(--text-primary);
}

.modal-close {
    background: none;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    color: var(--text-secondary);
    width: 32px;
    height: 32px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: all 0.2s;
}

.modal-close:hover {
    background: var(--error-color);
    color: var(--white);
}

.modal-form {
    padding: var(--spacing-md);
}

.form-group {
    margin-bottom: var(--spacing-md);
}

.form-group label {
    display: block;
    margin-bottom: var(--spacing-xs);
    font-weight: 500;
    color: var(--text-primary);
}

.form-group input,
.form-group select,
.form-group textarea {
    width: 100%;
    padding: var(--spacing-sm);
    border: 1px solid #ddd;
    border-radius: var(--border-radius);
    font-size: 1rem;
    transition: border-color 0.2s;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
    outline: none;
    border-color: var(--primary-color);
}

.form-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--spacing-md);
}

.modal-actions {
    display: flex;
    justify-content: flex-end;
    gap: var(--spacing-sm);
    margin-top: var(--spacing-lg);
    padding-top: var(--spacing-md);
    border-top: 1px solid #eee;
}

/* Buttons */
.btn-primary {
    background: var(--primary-color);
    color: var(--white);
    border: none;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: 0.9rem;
    font-weight: 500;
    transition: all 0.2s;
}

.btn-primary:hover {
    background: var(--argentina-blue);
    transform: translateY(-1px);
}

.btn-secondary {
    background: var(--secondary-color);
    color: var(--text-primary);
    border: 1px solid #ddd;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: 0.9rem;
    transition: all 0.2s;
}

.btn-secondary:hover {
    background: #e0e0e0;
    transform: translateY(-1px);
}

.btn-success {
    background: var(--success-color);
    color: var(--white);
    border: none;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: 0.9rem;
    transition: all 0.2s;
}

.btn-success:hover {
    background: #45a049;
    transform: translateY(-1px);
}

.btn-danger {
    background: var(--error-color);
    color: var(--white);
    border: none;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: 0.9rem;
    transition: all 0.2s;
}

.btn-danger:hover {
    background: #d32f2f;
    transform: translateY(-1px);
}

/* Invoice Services */
.invoice-services {
    margin-bottom: var(--spacing-md);
}

.service-row {
    display: grid;
    grid-template-columns: 2fr 1fr 1fr auto;
    gap: var(--spacing-sm);
    margin-bottom: var(--spacing-sm);
    align-items: center;
}

.service-select {
    padding: var(--spacing-xs);
}

.btn-remove {
    background: var(--error-color);
    color: var(--white);
    border: none;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.2s;
}

.btn-remove:hover {
    background: #d32f2f;
    transform: scale(1.1);
}

.btn-add {
    background: var(--success-color);
    color: var(--white);
    border: none;
    padding: var(--spacing-sm);
    border-radius: var(--border-radius);
    cursor: pointer;
    margin-bottom: var(--spacing-md);
    transition: all 0.2s;
    width: 100%;
}

.btn-add:hover {
    background: #45a049;
    transform: translateY(-1px);
}

.invoice-total {
    background: var(--secondary-color);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    margin-bottom: var(--spacing-md);
    border: 2px solid var(--primary-color);
}

.total-row {
    display: flex;
    justify-content: space-between;
    margin-bottom: var(--spacing-xs);
}

.total-row.total {
    font-weight: 600;
    font-size: 1.1rem;
    border-top: 1px solid #ddd;
    padding-top: var(--spacing-xs);
    color: var(--primary-color);
}

/* Client Profile Modal */
.client-profile-content {
    padding: var(--spacing-md);
}

.client-info {
    margin-bottom: var(--spacing-lg);
}

.info-row {
    display: flex;
    justify-content: space-between;
    margin-bottom: var(--spacing-sm);
    padding-bottom: var(--spacing-sm);
    border-bottom: 1px solid #eee;
}

.amount {
    color: var(--primary-color);
    font-weight: 600;
}

.client-actions {
    display: flex;
    gap: var(--spacing-sm);
}

/* Invoice Detail Modal */
.invoice-detail-content {
    padding: var(--spacing-md);
}

.invoice-header {
    margin-bottom: var(--spacing-lg);
}

.invoice-services-detail {
    margin-bottom: var(--spacing-lg);
}

.invoice-services-detail h4 {
    margin-bottom: var(--spacing-md);
    color: var(--text-primary);
}

.service-detail-item {
    display: flex;
    justify-content: space-between;
    padding: var(--spacing-sm);
    background: var(--secondary-color);
    margin-bottom: var(--spacing-xs);
    border-radius: var(--border-radius);
}

.invoice-totals {
    background: var(--secondary-color);
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    margin-bottom: var(--spacing-lg);
    border: 2px solid var(--primary-color);
}

.invoice-notes {
    margin-bottom: var(--spacing-lg);
}

.invoice-notes h4 {
    margin-bottom: var(--spacing-sm);
    color: var(--text-primary);
}

.invoice-actions {
    display: flex;
    flex-wrap: wrap;
    gap: var(--spacing-sm);
}

/* Service Detail Modal */
.service-detail-content {
    padding: var(--spacing-md);
}

.service-stats {
    margin-bottom: var(--spacing-lg);
}

.service-stats h4 {
    margin-bottom: var(--spacing-md);
    color: var(--text-primary);
}

.stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--spacing-md);
}

.stat-card {
    text-align: center;
    padding: var(--spacing-md);
    background: var(--secondary-color);
    border-radius: var(--border-radius);
    border: 2px solid var(--success-color);
}

.stat-value {
    font-size: 1.5rem;
    font-weight: 600;
    color: var(--primary-color);
}

.stat-label {
    font-size: 0.9rem;
    color: var(--text-secondary);
}

.service-actions {
    display: flex;
    gap: var(--spacing-sm);
}

/* Responsive Design */
@media (max-width: 768px) {
    .form-row {
        grid-template-columns: 1fr;
    }

    .service-row {
        grid-template-columns: 1fr;
        gap: var(--spacing-xs);
    }

    .metrics-grid {
        grid-template-columns: repeat(2, 1fr);
    }

    .quick-actions-grid {
        grid-template-columns: repeat(2, 1fr);
    }

    .invoice-actions,
    .client-actions,
    .service-actions {
        flex-direction: column;
    }

    .export-buttons {
        flex-direction: column;
    }

    .export-btn {
        min-width: auto;
    }

    .stats-grid {
        grid-template-columns: 1fr;
    }

    .bottom-nav {
        overflow-x: auto;
    }

    .nav-item {
        min-width: 60px;
    }
}

@media (max-width: 480px) {
    .metrics-grid {
        grid-template-columns: 1fr;
    }

    .filter-tabs {
        overflow-x: auto;
    }

    .modal-content {
        margin: var(--spacing-xs);
        max-width: calc(100vw - 16px);
    }

    .quick-actions-grid {
        grid-template-columns: 1fr;
    }

    .report-table {
        font-size: 0.8rem;
    }

    .service-stats,
    .client-stats {
        flex-direction: column;
        gap: var(--spacing-xs);
    }
}

/* Print Styles */
@media print {
    .header,
    .bottom-nav,
    .fab,
    .modal {
        display: none !important;
    }

    .main-content {
        padding: 0;
    }

    .screen {
        display: block !important;
    }
}

/* High Contrast Mode */
@media (prefers-contrast: high) {
    :root {
        --shadow: 0 2px 4px rgba(0,0,0,0.3);
        --shadow-lg: 0 4px 12px rgba(0,0,0,0.4);
    }

    .metric-card,
    .client-item,
    .invoice-item,
    .service-item {
        border: 2px solid var(--text-primary);
    }
}

/* Reduced Motion */
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
[app_mejorado.js](https://github.com/user-attachments/files/22480752/app_mejorado.js)

// Finance Control App - JavaScript (Versión Mejorada para Argentina)
class FinanceApp {
    constructor() {
        this.data = this.loadData();
        this.currentInvoiceFilter = 'todas';
        this.currentClient = null;
        this.currentInvoice = null;
        this.currentService = null;
        this.incomeChart = null;
        this.serviceChart = null;
        this.editingClient = null;
        this.editingInvoice = null;
        this.editingService = null;

        this.init();
    }

    // Initialize the app
    init() {
        this.renderDashboard();
        this.renderClients();
        this.renderInvoices();
        this.renderServices();
        this.renderReports();
        this.setupEventListeners();
        this.initChart();
    }

    // Load data from localStorage or use default data
    loadData() {
        const savedData = localStorage.getItem('financeAppData');
        if (savedData) {
            return JSON.parse(savedData);
        }

        // Default data con pesos argentinos
        return {
            clientes: [
                {id: 1, nombre: "Juan Pérez", telefono: "+54 11 1234-5678", email: "juan@email.com", direccion: "Av. Corrientes 1234, CABA", facturas: 5, totalFacturado: 425000},
                {id: 2, nombre: "María García", telefono: "+54 11 8765-4321", email: "maria@email.com", direccion: "Av. Santa Fe 2345, CABA", facturas: 3, totalFacturado: 289000},
                {id: 3, nombre: "Carlos López", telefono: "+54 11 3456-7890", email: "carlos@email.com", direccion: "Av. Rivadavia 5678, CABA", facturas: 7, totalFacturado: 615000},
                {id: 4, nombre: "Ana Martínez", telefono: "+54 11 9012-3456", email: "ana@email.com", direccion: "Av. Cabildo 3456, CABA", facturas: 2, totalFacturado: 175000},
                {id: 5, nombre: "Pedro Rodríguez", telefono: "+54 11 5678-9012", email: "pedro@email.com", direccion: "Av. Las Heras 7890, CABA", facturas: 4, totalFacturado: 345000}
            ],
            facturas: [
                {id: "001", cliente: "Juan Pérez", monto: 125000, estado: "Pendiente", fecha: "2025-09-15", vencimiento: "2025-10-15", servicios: [{nombre: "Consultoría Web", cantidad: 8, precio: 15000}], notas: "Desarrollo de sitio web corporativo"},
                {id: "002", cliente: "María García", monto: 89000, estado: "Pagada", fecha: "2025-09-12", vencimiento: "2025-10-12", servicios: [{nombre: "Diseño Gráfico", cantidad: 12, precio: 7500}], notas: "Material promocional"},
                {id: "003", cliente: "Carlos López", monto: 215000, estado: "Vencida", fecha: "2025-09-05", vencimiento: "2025-10-05", servicios: [{nombre: "Marketing Digital", cantidad: 2, precio: 80000}, {nombre: "Auditoría SEO", cantidad: 1, precio: 50000}], notas: "Campaña integral de marketing"},
                {id: "004", cliente: "Ana Martínez", monto: 75000, estado: "Pagada", fecha: "2025-09-18", vencimiento: "2025-10-18", servicios: [{nombre: "Diseño Gráfico", cantidad: 10, precio: 7500}], notas: "Rediseño de identidad visual"},
                {id: "005", cliente: "Pedro Rodríguez", monto: 145000, estado: "Pendiente", fecha: "2025-09-20", vencimiento: "2025-10-20", servicios: [{nombre: "Consultoría Web", cantidad: 8, precio: 15000}, {nombre: "Mantenimiento Web", cantidad: 1, precio: 20000}], notas: "Desarrollo y mantenimiento web"}
            ],
            servicios: [
                {id: 1, nombre: "Consultoría Web", precio: 15000, unidad: "hora", descripcion: "Desarrollo y diseño web profesional", vecesVendido: 24, ingresosGenerados: 360000},
                {id: 2, nombre: "Marketing Digital", precio: 80000, unidad: "mes", descripcion: "Gestión completa de redes sociales y campañas", vecesVendido: 8, ingresosGenerados: 640000},
                {id: 3, nombre: "Diseño Gráfico", precio: 7500, unidad: "hora", descripcion: "Creación de material visual y branding", vecesVendido: 32, ingresosGenerados: 240000},
                {id: 4, nombre: "Auditoría SEO", precio: 50000, unidad: "proyecto", descripcion: "Análisis completo y optimización SEO", vecesVendido: 12, ingresosGenerados: 600000},
                {id: 5, nombre: "Mantenimiento Web", precio: 20000, unidad: "mes", descripcion: "Soporte técnico y actualizaciones continuas", vecesVendido: 15, ingresosGenerados: 300000}
            ],
            metricas: {
                ingresosMesActual: 4575000,
                ingresosMesAnterior: 3820000,
                facturasPendientes: 12,
                facturasPagadas: 28,
                facturasVencidas: 3,
                totalClientes: 25,
                totalFacturas: 43,
                serviciosActivos: 15
            },
            ingresosMensuales: [
                {mes: "Enero", valor: 3200000, facturas: 15, clientes: 8},
                {mes: "Febrero", valor: 2850000, facturas: 12, clientes: 6},
                {mes: "Marzo", valor: 4120000, facturas: 18, clientes: 12},
                {mes: "Abril", valor: 3890000, facturas: 16, clientes: 10},
                {mes: "Mayo", valor: 4210000, facturas: 19, clientes: 11},
                {mes: "Junio", valor: 4575000, facturas: 21, clientes: 13}
            ],
            configuracion: {
                moneda: "ARS",
                simboloMoneda: "$",
                iva: 21,
                nombreEmpresa: "Mi Empresa SRL",
                cuit: "20-12345678-9",
                direccion: "Av. Corrientes 1234, CABA",
                telefono: "+54 11 1234-5678",
                email: "contacto@miempresa.com"
            }
        };
    }

    // Save data to localStorage
    saveData() {
        localStorage.setItem('financeAppData', JSON.stringify(this.data));
    }

    // Setup event listeners
    setupEventListeners() {
        // Client search
        const clientSearch = document.getElementById('client-search');
        if (clientSearch) {
            clientSearch.addEventListener('input', (e) => {
                this.searchClients(e.target.value);
            });
        }

        // Form submissions
        document.getElementById('newClientForm')?.addEventListener('submit', (e) => {
            e.preventDefault();
            this.addClient(new FormData(e.target));
        });

        document.getElementById('editClientForm')?.addEventListener('submit', (e) => {
            e.preventDefault();
            this.updateClient(new FormData(e.target));
        });

        document.getElementById('newInvoiceForm')?.addEventListener('submit', (e) => {
            e.preventDefault();
            this.addInvoice(new FormData(e.target));
        });

        document.getElementById('editInvoiceForm')?.addEventListener('submit', (e) => {
            e.preventDefault();
            this.updateInvoice(new FormData(e.target));
        });

        document.getElementById('newServiceForm')?.addEventListener('submit', (e) => {
            e.preventDefault();
            this.addService(new FormData(e.target));
        });

        document.getElementById('editServiceForm')?.addEventListener('submit', (e) => {
            e.preventDefault();
            this.updateService(new FormData(e.target));
        });

        // Service input listeners for invoice calculation
        document.addEventListener('input', (e) => {
            if (e.target.matches('input[name*="cantidad"], input[name*="precio"]')) {
                this.calculateInvoiceTotal();
            }
        });

        // Service select change
        document.addEventListener('change', (e) => {
            if (e.target.matches('select[name*="servicios"]')) {
                this.updateServicePrice(e.target);
            }
        });
    }

    // Initialize Chart
    initChart() {
        const ctx = document.getElementById('incomeChart')?.getContext('2d');
        if (ctx) {
            if (this.incomeChart) {
                this.incomeChart.destroy();
            }

            this.incomeChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: this.data.ingresosMensuales.map(m => m.mes),
                    datasets: [{
                        label: 'Ingresos ($)',
                        data: this.data.ingresosMensuales.map(m => m.valor),
                        backgroundColor: '#2196F3',
                        borderColor: '#1976D2',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                callback: function(value) {
                                    return '$' + value.toLocaleString('es-AR');
                                }
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return 'Ingresos: $' + context.parsed.y.toLocaleString('es-AR');
                                }
                            }
                        }
                    }
                }
            });
        }
    }

    // Switch between tabs/screens
    switchTab(tabName) {
        // Hide all screens
        document.querySelectorAll('.screen').forEach(screen => {
            screen.classList.remove('active');
        });

        // Show selected screen
        document.getElementById(tabName)?.classList.add('active');

        // Update navigation
        document.querySelectorAll('.nav-item').forEach(nav => {
            nav.classList.remove('active');
        });
        document.querySelector(`[data-tab="${tabName}"]`)?.classList.add('active');

        // Update page title
        const titles = {
            dashboard: 'Control de Finanzas',
            clientes: 'Mis Clientes',
            facturas: 'Gestión de Facturas',
            servicios: 'Catálogo de Servicios',
            reportes: 'Reportes y Análisis',
            config: 'Configuración'
        };

        document.getElementById('page-title').textContent = titles[tabName] || 'Control de Finanzas';

        // Render specific content if needed
        if (tabName === 'reportes') {
            this.renderReports();
        }
    }

    // Show modal
    showModal(modalId) {
        const modal = document.getElementById(modalId);
        if (modal) {
            modal.classList.add('active');

            // Populate client selector in new invoice modal
            if (modalId === 'newInvoiceModal' || modalId === 'editInvoiceModal') {
                this.populateClientSelector(modalId);
                this.populateServiceSelectors(modalId);
                if (modalId === 'newInvoiceModal') {
                    this.setDefaultDates();
                }
            }
        }
    }

    // Hide modal
    hideModal(modalId) {
        const modal = document.getElementById(modalId);
        if (modal) {
            modal.classList.remove('active');
            // Reset forms
            modal.querySelector('form')?.reset();
        }
    }

    // Render Dashboard
    renderDashboard() {
        this.updateMetrics();
    }

    // Update metrics
    updateMetrics() {
        const facturasPendientes = this.data.facturas.filter(f => f.estado === 'Pendiente').length;
        const facturasPagadas = this.data.facturas.filter(f => f.estado === 'Pagada').length;
        const facturasVencidas = this.data.facturas.filter(f => f.estado === 'Vencida').length;

        // Update counters
        const allCount = this.data.facturas.length;
        document.getElementById('all-count').textContent = allCount;
        document.getElementById('paid-count').textContent = facturasPagadas;
        document.getElementById('pending-count').textContent = facturasPendientes;
        document.getElementById('overdue-count').textContent = facturasVencidas;

        document.getElementById('client-count').textContent = this.data.clientes.length;
        document.getElementById('service-count').textContent = this.data.servicios.length;
    }

    // Render Clients
    renderClients() {
        const clientList = document.getElementById('client-list');
        if (!clientList) return;

        clientList.innerHTML = this.data.clientes.map(client => `
            <div class="client-item" onclick="app.showClientProfile(${client.id})">
                <div class="client-name">${client.nombre}</div>
                <div class="client-info">${client.telefono} • ${client.email}</div>
                <div class="client-actions">
                    <span class="client-invoices">${client.facturas} facturas</span>
                    <button class="view-profile-btn" onclick="event.stopPropagation(); app.showClientProfile(${client.id})">
                        Ver Perfil
                    </button>
                </div>
            </div>
        `).join('');
    }

    // Search clients
    searchClients(query) {
        const clientList = document.getElementById('client-list');
        if (!clientList) return;

        const filteredClients = this.data.clientes.filter(client => 
            client.nombre.toLowerCase().includes(query.toLowerCase()) ||
            client.email.toLowerCase().includes(query.toLowerCase()) ||
            client.telefono.includes(query)
        );

        clientList.innerHTML = filteredClients.map(client => `
            <div class="client-item" onclick="app.showClientProfile(${client.id})">
                <div class="client-name">${client.nombre}</div>
                <div class="client-info">${client.telefono} • ${client.email}</div>
                <div class="client-actions">
                    <span class="client-invoices">${client.facturas} facturas</span>
                    <button class="view-profile-btn" onclick="event.stopPropagation(); app.showClientProfile(${client.id})">
                        Ver Perfil
                    </button>
                </div>
            </div>
        `).join('');
    }

    // Show client profile
    showClientProfile(clientId) {
        const client = this.data.clientes.find(c => c.id === clientId);
        if (!client) return;

        this.currentClient = client;

        document.getElementById('client-profile-name').textContent = client.nombre;
        document.getElementById('client-profile-phone').textContent = client.telefono;
        document.getElementById('client-profile-email').textContent = client.email;
        document.getElementById('client-profile-address').textContent = client.direccion;
        document.getElementById('client-profile-total').textContent = '$' + client.totalFacturado.toLocaleString('es-AR');
        document.getElementById('client-profile-invoices').textContent = client.facturas;

        this.showModal('clientProfileModal');
    }

    // Add new client
    addClient(formData) {
        const newClient = {
            id: Math.max(...this.data.clientes.map(c => c.id), 0) + 1,
            nombre: formData.get('nombre'),
            telefono: formData.get('telefono'),
            email: formData.get('email'),
            direccion: formData.get('direccion') || '',
            facturas: 0,
            totalFacturado: 0
        };

        this.data.clientes.push(newClient);
        this.saveData();
        this.renderClients();
        this.updateMetrics();
        this.hideModal('newClientModal');

        this.showNotification('Cliente agregado exitosamente', 'success');
    }

    // Edit client - Show edit modal
    editClient() {
        if (!this.currentClient) return;

        this.editingClient = this.currentClient;

        // Populate edit form
        document.querySelector('#editClientForm input[name="nombre"]').value = this.currentClient.nombre;
        document.querySelector('#editClientForm input[name="telefono"]').value = this.currentClient.telefono;
        document.querySelector('#editClientForm input[name="email"]').value = this.currentClient.email;
        document.querySelector('#editClientForm textarea[name="direccion"]').value = this.currentClient.direccion;

        this.hideModal('clientProfileModal');
        this.showModal('editClientModal');
    }

    // Update client
    updateClient(formData) {
        if (!this.editingClient) return;

        const clientIndex = this.data.clientes.findIndex(c => c.id === this.editingClient.id);
        if (clientIndex === -1) return;

        this.data.clientes[clientIndex] = {
            ...this.data.clientes[clientIndex],
            nombre: formData.get('nombre'),
            telefono: formData.get('telefono'),
            email: formData.get('email'),
            direccion: formData.get('direccion') || ''
        };

        // Update invoices with new client name
        this.data.facturas.forEach(factura => {
            if (factura.cliente === this.editingClient.nombre) {
                factura.cliente = this.data.clientes[clientIndex].nombre;
            }
        });

        this.saveData();
        this.renderClients();
        this.renderInvoices();
        this.hideModal('editClientModal');

        this.showNotification('Cliente actualizado exitosamente', 'success');
    }

    // Delete client
    deleteClient() {
        if (!this.currentClient) return;

        if (confirm('¿Está seguro de que desea eliminar este cliente? Esta acción también eliminará todas sus facturas asociadas.')) {
            // Remove client's invoices
            this.data.facturas = this.data.facturas.filter(f => f.cliente !== this.currentClient.nombre);

            // Remove client
            this.data.clientes = this.data.clientes.filter(c => c.id !== this.currentClient.id);

            this.saveData();
            this.renderClients();
            this.renderInvoices();
            this.updateMetrics();
            this.hideModal('clientProfileModal');
            this.showNotification('Cliente eliminado exitosamente', 'success');
        }
    }

    // Render Invoices
    renderInvoices() {
        const invoiceList = document.getElementById('invoice-list');
        if (!invoiceList) return;

        let filteredInvoices = this.data.facturas;

        if (this.currentInvoiceFilter !== 'todas') {
            filteredInvoices = this.data.facturas.filter(invoice => invoice.estado === this.currentInvoiceFilter);
        }

        invoiceList.innerHTML = filteredInvoices.map(invoice => `
            <div class="invoice-item" onclick="app.showInvoiceDetail('${invoice.id}')">
                <div class="invoice-header">
                    <div class="invoice-number">#${invoice.id}</div>
                    <div class="invoice-amount">$${invoice.monto.toLocaleString('es-AR')}</div>
                </div>
                <div class="invoice-client">${invoice.cliente}</div>
                <div class="invoice-footer">
                    <div class="invoice-date">${this.formatDate(invoice.fecha)}</div>
                    <span class="status-badge ${invoice.estado}">${invoice.estado}</span>
                </div>
            </div>
        `).join('');
    }

    // Filter invoices
    filterInvoices(filter) {
        this.currentInvoiceFilter = filter;

        // Update active filter tab
        document.querySelectorAll('.filter-tab').forEach(tab => {
            tab.classList.remove('active');
        });
        document.querySelector(`[data-filter="${filter}"]`)?.classList.add('active');

        this.renderInvoices();
    }

    // Show invoice detail
    showInvoiceDetail(invoiceId) {
        const invoice = this.data.facturas.find(i => i.id === invoiceId);
        if (!invoice) return;

        this.currentInvoice = invoice;

        document.getElementById('invoice-detail-title').textContent = `Factura #${invoice.id}`;
        document.getElementById('invoice-detail-number').textContent = invoice.id;
        document.getElementById('invoice-detail-client').textContent = invoice.cliente;
        document.getElementById('invoice-detail-date').textContent = this.formatDate(invoice.fecha);
        document.getElementById('invoice-detail-due').textContent = this.formatDate(invoice.vencimiento);

        const statusBadge = document.getElementById('invoice-detail-status');
        statusBadge.textContent = invoice.estado;
        statusBadge.className = `status-badge ${invoice.estado}`;

        // Render services
        const servicesContainer = document.getElementById('invoice-detail-services');
        servicesContainer.innerHTML = invoice.servicios.map(service => `
            <div class="service-detail-item">
                <span>${service.nombre} (${service.cantidad})</span>
                <span>$${(service.cantidad * service.precio).toLocaleString('es-AR')}</span>
            </div>
        `).join('');

        // Calculate totals
        const subtotal = invoice.servicios.reduce((sum, service) => sum + (service.cantidad * service.precio), 0);
        const tax = subtotal * (this.data.configuracion.iva / 100);
        const total = subtotal + tax;

        document.getElementById('invoice-detail-subtotal').textContent = '$' + subtotal.toLocaleString('es-AR');
        document.getElementById('invoice-detail-tax').textContent = '$' + tax.toFixed(0);
        document.getElementById('invoice-detail-total').textContent = '$' + total.toFixed(0);

        // Notes
        const notesSection = document.getElementById('invoice-detail-notes-section');
        const notesText = document.getElementById('invoice-detail-notes');
        if (invoice.notas) {
            notesSection.style.display = 'block';
            notesText.textContent = invoice.notas;
        } else {
            notesSection.style.display = 'none';
        }

        // Show/hide mark as paid button
        const markPaidBtn = document.getElementById('mark-paid-btn');
        markPaidBtn.style.display = invoice.estado === 'Pagada' ? 'none' : 'inline-block';

        this.showModal('invoiceDetailModal');
    }

    // Mark invoice as paid
    markAsPaid() {
        if (this.currentInvoice) {
            this.currentInvoice.estado = 'Pagada';
            this.saveData();
            this.renderInvoices();
            this.updateMetrics();
            this.hideModal('invoiceDetailModal');
            this.showNotification('Factura marcada como pagada', 'success');
        }
    }

    // Edit invoice - Show edit modal
    editInvoice() {
        if (!this.currentInvoice) return;

        this.editingInvoice = this.currentInvoice;

        // Populate edit form
        document.querySelector('#editInvoiceForm select[name="cliente"]').value = this.currentInvoice.cliente;
        document.querySelector('#editInvoiceForm input[name="fecha"]').value = this.currentInvoice.fecha;
        document.querySelector('#editInvoiceForm input[name="vencimiento"]').value = this.currentInvoice.vencimiento;
        document.querySelector('#editInvoiceForm select[name="estado"]').value = this.currentInvoice.estado;
        document.querySelector('#editInvoiceForm textarea[name="notas"]').value = this.currentInvoice.notas || '';

        // Populate services
        const servicesContainer = document.getElementById('edit-invoice-services');
        servicesContainer.innerHTML = '';

        this.currentInvoice.servicios.forEach((service, index) => {
            this.addServiceRowToEdit(index, service);
        });

        this.hideModal('invoiceDetailModal');
        this.showModal('editInvoiceModal');
    }

    // Add service row to edit form
    addServiceRowToEdit(index, service = null) {
        const container = document.getElementById('edit-invoice-services');

        const serviceOptions = '<option value="">Seleccionar servicio</option>' + 
            this.data.servicios.map(s => `<option value="${s.nombre}" data-price="${s.precio}" ${service && service.nombre === s.nombre ? 'selected' : ''}>${s.nombre}</option>`).join('');

        const newRow = document.createElement('div');
        newRow.className = 'service-row';
        newRow.innerHTML = `
            <select name="servicios[${index}][nombre]" class="service-select" onchange="app.updateServicePriceEdit(this)">
                ${serviceOptions}
            </select>
            <input type="number" name="servicios[${index}][cantidad]" placeholder="Cantidad" min="1" value="${service ? service.cantidad : 1}">
            <input type="number" name="servicios[${index}][precio]" placeholder="Precio" step="0.01" min="0" value="${service ? service.precio : ''}">
            <button type="button" class="btn-remove" onclick="app.removeServiceRowEdit(this)">-</button>
        `;

        container.appendChild(newRow);
    }

    // Update service price in edit form
    updateServicePriceEdit(selectElement) {
        const selectedOption = selectElement.selectedOptions[0];
        if (selectedOption && selectedOption.dataset.price) {
            const row = selectElement.closest('.service-row');
            const priceInput = row.querySelector('input[name*="precio"]');
            priceInput.value = selectedOption.dataset.price;
            this.calculateEditInvoiceTotal();
        }
    }

    // Remove service row from edit form
    removeServiceRowEdit(button) {
        const row = button.closest('.service-row');
        if (row.parentElement.children.length > 1) {
            row.remove();
            this.calculateEditInvoiceTotal();
        }
    }

    // Calculate edit invoice total
    calculateEditInvoiceTotal() {
        const serviceRows = document.querySelectorAll('#edit-invoice-services .service-row');
        let subtotal = 0;

        serviceRows.forEach(row => {
            const cantidad = parseFloat(row.querySelector('input[name*="cantidad"]').value) || 0;
            const precio = parseFloat(row.querySelector('input[name*="precio"]').value) || 0;
            subtotal += cantidad * precio;
        });

        const tax = subtotal * (this.data.configuracion.iva / 100);
        const total = subtotal + tax;

        document.getElementById('edit-invoice-subtotal').textContent = '$' + subtotal.toFixed(0);
        document.getElementById('edit-invoice-tax').textContent = '$' + tax.toFixed(0);
        document.getElementById('edit-invoice-total').textContent = '$' + total.toFixed(0);
    }

    // Update invoice
    updateInvoice(formData) {
        if (!this.editingInvoice) return;

        const serviceRows = document.querySelectorAll('#edit-invoice-services .service-row');
        const servicios = [];

        serviceRows.forEach((row, index) => {
            const nombre = row.querySelector('select').value;
            const cantidad = parseFloat(row.querySelector('input[name*="cantidad"]').value) || 0;
            const precio = parseFloat(row.querySelector('input[name*="precio"]').value) || 0;

            if (nombre && cantidad > 0 && precio > 0) {
                servicios.push({ nombre, cantidad, precio });
            }
        });

        if (servicios.length === 0) {
            this.showNotification('Debe agregar al menos un servicio', 'error');
            return;
        }

        const subtotal = servicios.reduce((sum, service) => sum + (service.cantidad * service.precio), 0);
        const tax = subtotal * (this.data.configuracion.iva / 100);
        const total = subtotal + tax;

        const invoiceIndex = this.data.facturas.findIndex(f => f.id === this.editingInvoice.id);
        if (invoiceIndex === -1) return;

        // Update client totals
        const oldClient = this.data.clientes.find(c => c.nombre === this.editingInvoice.cliente);
        const newClient = this.data.clientes.find(c => c.nombre === formData.get('cliente'));

        if (oldClient && oldClient !== newClient) {
            oldClient.facturas--;
            oldClient.totalFacturado -= this.editingInvoice.monto;
        }

        if (newClient && oldClient !== newClient) {
            newClient.facturas++;
            newClient.totalFacturado += Math.round(total);
        } else if (newClient) {
            newClient.totalFacturado = newClient.totalFacturado - this.editingInvoice.monto + Math.round(total);
        }

        this.data.facturas[invoiceIndex] = {
            ...this.data.facturas[invoiceIndex],
            cliente: formData.get('cliente'),
            fecha: formData.get('fecha'),
            vencimiento: formData.get('vencimiento'),
            estado: formData.get('estado'),
            servicios: servicios,
            notas: formData.get('notas') || '',
            monto: Math.round(total)
        };

        this.saveData();
        this.renderInvoices();
        this.renderClients();
        this.updateMetrics();
        this.hideModal('editInvoiceModal');

        this.showNotification('Factura actualizada exitosamente', 'success');
    }

    // Delete invoice
    deleteInvoice() {
        if (!this.currentInvoice) return;

        if (confirm('¿Está seguro de que desea eliminar esta factura?')) {
            // Update client totals
            const client = this.data.clientes.find(c => c.nombre === this.currentInvoice.cliente);
            if (client) {
                client.facturas--;
                client.totalFacturado -= this.currentInvoice.monto;
            }

            this.data.facturas = this.data.facturas.filter(f => f.id !== this.currentInvoice.id);
            this.saveData();
            this.renderInvoices();
            this.renderClients();
            this.updateMetrics();
            this.hideModal('invoiceDetailModal');
            this.showNotification('Factura eliminada exitosamente', 'success');
        }
    }

    // Populate client selector
    populateClientSelector(modalId = 'newInvoiceModal') {
        const clientSelect = document.querySelector(`#${modalId.replace('Modal', 'Form')} select[name="cliente"]`);
        if (clientSelect) {
            clientSelect.innerHTML = '<option value="">Seleccionar cliente</option>' + 
                this.data.clientes.map(client => `<option value="${client.nombre}">${client.nombre}</option>`).join('');
        }
    }

    // Populate service selectors
    populateServiceSelectors(modalId = 'newInvoiceModal') {
        const formId = modalId.replace('Modal', 'Form');
        const serviceSelects = document.querySelectorAll(`#${formId} .service-select`);
        const options = '<option value="">Seleccionar servicio</option>' + 
            this.data.servicios.map(service => `<option value="${service.nombre}" data-price="${service.precio}">${service.nombre}</option>`).join('');

        serviceSelects.forEach(select => {
            select.innerHTML = options;
        });
    }

    // Update service price
    updateServicePrice(selectElement) {
        const selectedOption = selectElement.selectedOptions[0];
        if (selectedOption && selectedOption.dataset.price) {
            const row = selectElement.closest('.service-row');
            const priceInput = row.querySelector('input[name*="precio"]');
            priceInput.value = selectedOption.dataset.price;
            this.calculateInvoiceTotal();
        }
    }

    // Add service row
    addServiceRow() {
        const container = document.getElementById('invoice-services');
        const rowCount = container.children.length;

        const serviceOptions = '<option value="">Seleccionar servicio</option>' + 
            this.data.servicios.map(service => `<option value="${service.nombre}" data-price="${service.precio}">${service.nombre}</option>`).join('');

        const newRow = document.createElement('div');
        newRow.className = 'service-row';
        newRow.innerHTML = `
            <select name="servicios[${rowCount}][nombre]" class="service-select" onchange="app.updateServicePrice(this)">
                ${serviceOptions}
            </select>
            <input type="number" name="servicios[${rowCount}][cantidad]" placeholder="Cantidad" min="1" value="1">
            <input type="number" name="servicios[${rowCount}][precio]" placeholder="Precio" step="0.01" min="0">
            <button type="button" class="btn-remove" onclick="app.removeServiceRow(this)">-</button>
        `;

        container.appendChild(newRow);
    }

    // Add service row to edit form
    addServiceRowToEditForm() {
        const container = document.getElementById('edit-invoice-services');
        const rowCount = container.children.length;

        this.addServiceRowToEdit(rowCount);
        this.populateServiceSelectors('editInvoiceModal');
    }

    // Remove service row
    removeServiceRow(button) {
        const row = button.closest('.service-row');
        if (row.parentElement.children.length > 1) {
            row.remove();
            this.calculateInvoiceTotal();
        }
    }

    // Calculate invoice total
    calculateInvoiceTotal() {
        const serviceRows = document.querySelectorAll('#invoice-services .service-row');
        let subtotal = 0;

        serviceRows.forEach(row => {
            const cantidad = parseFloat(row.querySelector('input[name*="cantidad"]').value) || 0;
            const precio = parseFloat(row.querySelector('input[name*="precio"]').value) || 0;
            subtotal += cantidad * precio;
        });

        const tax = subtotal * (this.data.configuracion.iva / 100);
        const total = subtotal + tax;

        document.getElementById('invoice-subtotal').textContent = '$' + subtotal.toFixed(0);
        document.getElementById('invoice-tax').textContent = '$' + tax.toFixed(0);
        document.getElementById('invoice-total').textContent = '$' + total.toFixed(0);
    }

    // Set default dates
    setDefaultDates() {
        const today = new Date();
        const nextMonth = new Date(today);
        nextMonth.setMonth(nextMonth.getMonth() + 1);

        const fechaInput = document.querySelector('#newInvoiceForm input[name="fecha"]');
        const vencimientoInput = document.querySelector('#newInvoiceForm input[name="vencimiento"]');

        if (fechaInput) fechaInput.value = this.formatDateForInput(today);
        if (vencimientoInput) vencimientoInput.value = this.formatDateForInput(nextMonth);
    }

    // Add new invoice
    addInvoice(formData) {
        const serviceRows = document.querySelectorAll('#invoice-services .service-row');
        const servicios = [];

        serviceRows.forEach((row, index) => {
            const nombre = row.querySelector('select').value;
            const cantidad = parseFloat(row.querySelector('input[name*="cantidad"]').value) || 0;
            const precio = parseFloat(row.querySelector('input[name*="precio"]').value) || 0;

            if (nombre && cantidad > 0 && precio > 0) {
                servicios.push({ nombre, cantidad, precio });
            }
        });

        if (servicios.length === 0) {
            this.showNotification('Debe agregar al menos un servicio', 'error');
            return;
        }

        const subtotal = servicios.reduce((sum, service) => sum + (service.cantidad * service.precio), 0);
        const tax = subtotal * (this.data.configuracion.iva / 100);
        const total = subtotal + tax;

        const newInvoice = {
            id: (Math.max(...this.data.facturas.map(f => parseInt(f.id)), 0) + 1).toString().padStart(3, '0'),
            cliente: formData.get('cliente'),
            monto: Math.round(total),
            estado: formData.get('estado'),
            fecha: formData.get('fecha'),
            vencimiento: formData.get('vencimiento'),
            servicios: servicios,
            notas: formData.get('notas') || ''
        };

        this.data.facturas.push(newInvoice);

        // Update client invoice count
        const client = this.data.clientes.find(c => c.nombre === newInvoice.cliente);
        if (client) {
            client.facturas++;
            client.totalFacturado += newInvoice.monto;
        }

        this.saveData();
        this.renderInvoices();
        this.renderClients();
        this.updateMetrics();
        this.hideModal('newInvoiceModal');

        this.showNotification('Factura creada exitosamente', 'success');
    }

    // Render Services
    renderServices() {
        const serviceList = document.getElementById('service-list');
        if (!serviceList) return;

        serviceList.innerHTML = this.data.servicios.map(service => `
            <div class="service-item" onclick="app.showServiceDetail(${service.id})">
                <div class="service-header">
                    <div class="service-name">${service.nombre}</div>
                    <div class="service-price">$${service.precio.toLocaleString('es-AR')}/${service.unidad}</div>
                </div>
                <div class="service-description">${service.descripcion}</div>
                <div class="service-stats">
                    <div>Vendido: ${service.vecesVendido} veces</div>
                    <div>Ingresos: $${service.ingresosGenerados.toLocaleString('es-AR')}</div>
                </div>
            </div>
        `).join('');
    }

    // Show service detail
    showServiceDetail(serviceId) {
        const service = this.data.servicios.find(s => s.id === serviceId);
        if (!service) return;

        this.currentService = service;

        document.getElementById('service-detail-name').textContent = service.nombre;
        document.getElementById('service-detail-price').textContent = '$' + service.precio.toLocaleString('es-AR') + '/' + service.unidad;
        document.getElementById('service-detail-unit').textContent = service.unidad;
        document.getElementById('service-detail-description').textContent = service.descripcion;
        document.getElementById('service-detail-sold').textContent = service.vecesVendido;
        document.getElementById('service-detail-revenue').textContent = '$' + service.ingresosGenerados.toLocaleString('es-AR');

        this.showModal('serviceDetailModal');
    }

    // Add new service
    addService(formData) {
        const newService = {
            id: Math.max(...this.data.servicios.map(s => s.id), 0) + 1,
            nombre: formData.get('nombre'),
            precio: parseFloat(formData.get('precio')),
            unidad: formData.get('unidad'),
            descripcion: formData.get('descripcion') || '',
            vecesVendido: 0,
            ingresosGenerados: 0
        };

        this.data.servicios.push(newService);
        this.saveData();
        this.renderServices();
        this.updateMetrics();
        this.hideModal('newServiceModal');

        this.showNotification('Servicio agregado exitosamente', 'success');
    }

    // Edit service
    editService() {
        if (!this.currentService) return;

        this.editingService = this.currentService;

        // Populate edit form
        document.querySelector('#editServiceForm input[name="nombre"]').value = this.currentService.nombre;
        document.querySelector('#editServiceForm input[name="precio"]').value = this.currentService.precio;
        document.querySelector('#editServiceForm select[name="unidad"]').value = this.currentService.unidad;
        document.querySelector('#editServiceForm textarea[name="descripcion"]').value = this.currentService.descripcion;

        this.hideModal('serviceDetailModal');
        this.showModal('editServiceModal');
    }

    // Update service
    updateService(formData) {
        if (!this.editingService) return;

        const serviceIndex = this.data.servicios.findIndex(s => s.id === this.editingService.id);
        if (serviceIndex === -1) return;

        const oldName = this.data.servicios[serviceIndex].nombre;
        const newName = formData.get('nombre');

        this.data.servicios[serviceIndex] = {
            ...this.data.servicios[serviceIndex],
            nombre: newName,
            precio: parseFloat(formData.get('precio')),
            unidad: formData.get('unidad'),
            descripcion: formData.get('descripcion') || ''
        };

        // Update invoices with new service name
        if (oldName !== newName) {
            this.data.facturas.forEach(factura => {
                factura.servicios.forEach(servicio => {
                    if (servicio.nombre === oldName) {
                        servicio.nombre = newName;
                    }
                });
            });
        }

        this.saveData();
        this.renderServices();
        this.renderInvoices();
        this.hideModal('editServiceModal');

        this.showNotification('Servicio actualizado exitosamente', 'success');
    }

    // Delete service
    deleteService() {
        if (!this.currentService) return;

        // Check if service is used in invoices
        const serviceUsed = this.data.facturas.some(factura => 
            factura.servicios.some(servicio => servicio.nombre === this.currentService.nombre)
        );

        if (serviceUsed) {
            if (!confirm('Este servicio está siendo usado en facturas existentes. Si lo elimina, se eliminará de todas las facturas. ¿Está seguro?')) {
                return;
            }

            // Remove service from all invoices
            this.data.facturas.forEach(factura => {
                factura.servicios = factura.servicios.filter(servicio => servicio.nombre !== this.currentService.nombre);

                // Recalculate invoice total
                if (factura.servicios.length > 0) {
                    const subtotal = factura.servicios.reduce((sum, service) => sum + (service.cantidad * service.precio), 0);
                    const tax = subtotal * (this.data.configuracion.iva / 100);
                    factura.monto = Math.round(subtotal + tax);
                } else {
                    // If no services left, mark invoice for deletion or set to 0
                    factura.monto = 0;
                }
            });

            // Remove empty invoices
            this.data.facturas = this.data.facturas.filter(factura => factura.servicios.length > 0 && factura.monto > 0);
        }

        this.data.servicios = this.data.servicios.filter(s => s.id !== this.currentService.id);
        this.saveData();
        this.renderServices();
        this.renderInvoices();
        this.renderClients();
        this.updateMetrics();
        this.hideModal('serviceDetailModal');
        this.showNotification('Servicio eliminado exitosamente', 'success');
    }

    // Render Reports
    renderReports() {
        this.renderMonthlySalesChart();
        this.renderMonthlySalesTable();
        this.renderTopServices();
        this.renderClientAnalysis();
    }

    // Render monthly sales chart
    renderMonthlySalesChart() {
        const ctx = document.getElementById('monthlyChart')?.getContext('2d');
        if (!ctx) return;

        if (this.monthlyChart) {
            this.monthlyChart.destroy();
        }

        this.monthlyChart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: this.data.ingresosMensuales.map(m => m.mes),
                datasets: [{
                    label: 'Ingresos',
                    data: this.data.ingresosMensuales.map(m => m.valor),
                    borderColor: '#2196F3',
                    backgroundColor: 'rgba(33, 150, 243, 0.1)',
                    borderWidth: 3,
                    fill: true,
                    tension: 0.4
                }, {
                    label: 'Facturas',
                    data: this.data.ingresosMensuales.map(m => m.facturas),
                    borderColor: '#4CAF50',
                    backgroundColor: 'rgba(76, 175, 80, 0.1)',
                    borderWidth: 2,
                    fill: false,
                    yAxisID: 'y1'
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        type: 'linear',
                        display: true,
                        position: 'left',
                        beginAtZero: true,
                        ticks: {
                            callback: function(value) {
                                return '$' + value.toLocaleString('es-AR');
                            }
                        }
                    },
                    y1: {
                        type: 'linear',
                        display: true,
                        position: 'right',
                        beginAtZero: true,
                        grid: {
                            drawOnChartArea: false,
                        },
                        ticks: {
                            callback: function(value) {
                                return value + ' facturas';
                            }
                        }
                    }
                },
                plugins: {
                    tooltip: {
                        callbacks: {
                            label: function(context) {
                                if (context.dataset.label === 'Ingresos') {
                                    return 'Ingresos: $' + context.parsed.y.toLocaleString('es-AR');
                                } else {
                                    return 'Facturas: ' + context.parsed.y;
                                }
                            }
                        }
                    }
                }
            }
        });
    }

    // Render monthly sales table
    renderMonthlySalesTable() {
        const tableBody = document.getElementById('monthly-sales-table');
        if (!tableBody) return;

        tableBody.innerHTML = this.data.ingresosMensuales.map(mes => `
            <tr>
                <td>${mes.mes}</td>
                <td>$${mes.valor.toLocaleString('es-AR')}</td>
                <td>${mes.facturas}</td>
                <td>${mes.clientes}</td>
                <td>$${Math.round(mes.valor / mes.facturas).toLocaleString('es-AR')}</td>
            </tr>
        `).join('');
    }

    // Render top services
    renderTopServices() {
        const container = document.getElementById('top-services');
        if (!container) return;

        const sortedServices = [...this.data.servicios]
            .sort((a, b) => b.ingresosGenerados - a.ingresosGenerados)
            .slice(0, 5);

        container.innerHTML = sortedServices.map((service, index) => `
            <div class="top-service-item">
                <div class="service-rank">#${index + 1}</div>
                <div class="service-info">
                    <div class="service-name">${service.nombre}</div>
                    <div class="service-stats">
                        <span>$${service.ingresosGenerados.toLocaleString('es-AR')}</span>
                        <span>${service.vecesVendido} ventas</span>
                    </div>
                </div>
            </div>
        `).join('');
    }

    // Render client analysis
    renderClientAnalysis() {
        const container = document.getElementById('client-analysis');
        if (!container) return;

        const sortedClients = [...this.data.clientes]
            .sort((a, b) => b.totalFacturado - a.totalFacturado)
            .slice(0, 5);

        container.innerHTML = sortedClients.map((client, index) => `
            <div class="top-client-item">
                <div class="client-rank">#${index + 1}</div>
                <div class="client-info">
                    <div class="client-name">${client.nombre}</div>
                    <div class="client-stats">
                        <span>$${client.totalFacturado.toLocaleString('es-AR')}</span>
                        <span>${client.facturas} facturas</span>
                    </div>
                </div>
            </div>
        `).join('');
    }

    // Export to Excel for accountant
    exportToExcel() {
        // Create comprehensive data for accountant
        const exportData = {
            resumenEjecutivo: {
                periodo: `${this.data.ingresosMensuales[0].mes} - ${this.data.ingresosMensuales[this.data.ingresosMensuales.length - 1].mes} 2025`,
                ingresoTotal: this.data.ingresosMensuales.reduce((sum, m) => sum + m.valor, 0),
                totalFacturas: this.data.facturas.length,
                totalClientes: this.data.clientes.length,
                facturasPendientes: this.data.facturas.filter(f => f.estado === 'Pendiente').length,
                facturasVencidas: this.data.facturas.filter(f => f.estado === 'Vencida').length
            },
            empresa: this.data.configuracion,
            ingresosMensuales: this.data.ingresosMensuales,
            facturas: this.data.facturas.map(f => ({
                ...f,
                subtotal: f.servicios.reduce((sum, s) => sum + (s.cantidad * s.precio), 0),
                iva: f.servicios.reduce((sum, s) => sum + (s.cantidad * s.precio), 0) * (this.data.configuracion.iva / 100),
                serviciosDetalle: f.servicios.map(s => `${s.nombre} (${s.cantidad} x $${s.precio})`).join('; ')
            })),
            clientes: this.data.clientes,
            servicios: this.data.servicios
        };

        // Create CSV content for Excel
        let csvContent = '';

        // Resumen Ejecutivo
        csvContent += 'RESUMEN EJECUTIVO\n';
        csvContent += `Empresa,${exportData.empresa.nombreEmpresa}\n`;
        csvContent += `CUIT,${exportData.empresa.cuit}\n`;
        csvContent += `Período,${exportData.resumenEjecutivo.periodo}\n`;
        csvContent += `Ingreso Total,$${exportData.resumenEjecutivo.ingresoTotal.toLocaleString('es-AR')}\n`;
        csvContent += `Total Facturas,${exportData.resumenEjecutivo.totalFacturas}\n`;
        csvContent += `Total Clientes,${exportData.resumenEjecutivo.totalClientes}\n`;
        csvContent += `Facturas Pendientes,${exportData.resumenEjecutivo.facturasPendientes}\n`;
        csvContent += `Facturas Vencidas,${exportData.resumenEjecutivo.facturasVencidas}\n\n`;

        // Ingresos Mensuales
        csvContent += 'INGRESOS MENSUALES\n';
        csvContent += 'Mes,Ingresos,Facturas,Clientes,Promedio por Factura\n';
        exportData.ingresosMensuales.forEach(mes => {
            csvContent += `${mes.mes},$${mes.valor.toLocaleString('es-AR')},${mes.facturas},${mes.clientes},$${Math.round(mes.valor/mes.facturas).toLocaleString('es-AR')}\n`;
        });
        csvContent += '\n';

        // Detalle de Facturas
        csvContent += 'DETALLE DE FACTURAS\n';
        csvContent += 'Número,Cliente,Fecha,Vencimiento,Estado,Subtotal,IVA,Total,Servicios,Notas\n';
        exportData.facturas.forEach(factura => {
            csvContent += `${factura.id},"${factura.cliente}",${factura.fecha},${factura.vencimiento},${factura.estado},$${factura.subtotal.toFixed(0)},$${factura.iva.toFixed(0)},$${factura.monto},"${factura.serviciosDetalle}","${factura.notas || ''}"\n`;
        });
        csvContent += '\n';

        // Clientes
        csvContent += 'CLIENTES\n';
        csvContent += 'Nombre,Teléfono,Email,Dirección,Total Facturado,Cantidad Facturas\n';
        exportData.clientes.forEach(cliente => {
            csvContent += `"${cliente.nombre}","${cliente.telefono}","${cliente.email}","${cliente.direccion}",$${cliente.totalFacturado.toLocaleString('es-AR')},${cliente.facturas}\n`;
        });
        csvContent += '\n';

        // Servicios
        csvContent += 'SERVICIOS\n';
        csvContent += 'Nombre,Precio,Unidad,Descripción,Veces Vendido,Ingresos Generados\n';
        exportData.servicios.forEach(servicio => {
            csvContent += `"${servicio.nombre}",$${servicio.precio.toLocaleString('es-AR')},${servicio.unidad},"${servicio.descripcion}",${servicio.vecesVendido},$${servicio.ingresosGenerados.toLocaleString('es-AR')}\n`;
        });

        // Create and download file
        const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
        const link = document.createElement('a');
        const url = URL.createObjectURL(blob);
        link.setAttribute('href', url);
        link.setAttribute('download', `informe-contable-${new Date().toISOString().split('T')[0]}.csv`);
        link.style.visibility = 'hidden';
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        URL.revokeObjectURL(url);

        this.showNotification('Informe para contador exportado exitosamente', 'success');
    }

    // Export data
    exportData() {
        const dataStr = JSON.stringify(this.data, null, 2);
        const dataBlob = new Blob([dataStr], {type: 'application/json'});
        const url = URL.createObjectURL(dataBlob);
        const link = document.createElement('a');
        link.href = url;
        link.download = 'finanzas-backup-' + new Date().toISOString().split('T')[0] + '.json';
        link.click();
        URL.revokeObjectURL(url);

        this.showNotification('Datos exportados exitosamente', 'success');
    }

    // Clear data
    clearData() {
        if (confirm('¿Está seguro de que desea limpiar todos los datos? Esta acción no se puede deshacer.')) {
            localStorage.removeItem('financeAppData');
            location.reload();
        }
    }

    // Share invoice (placeholder)
    shareInvoice() {
        this.showNotification('Compartir factura - función en desarrollo', 'info');
    }

    // Utility functions
    formatDate(dateString) {
        const date = new Date(dateString);
        return date.toLocaleDateString('es-AR');
    }

    formatDateForInput(date) {
        return date.toISOString().split('T')[0];
    }

    showNotification(message, type = 'info') {
        // Create notification element
        const notification = document.createElement('div');
        notification.className = `notification ${type}`;
        notification.textContent = message;

        // Style the notification
        Object.assign(notification.style, {
            position: 'fixed',
            top: '20px',
            right: '20px',
            padding: '12px 20px',
            borderRadius: '8px',
            color: 'white',
            fontWeight: '500',
            zIndex: '9999',
            transform: 'translateX(400px)',
            transition: 'transform 0.3s ease',
            maxWidth: '300px'
        });

        // Set background color based on type
        const colors = {
            success: '#4CAF50',
            error: '#F44336',
            warning: '#FF9800',
            info: '#2196F3'
        };
        notification.style.backgroundColor = colors[type] || colors.info;

        // Add to DOM
        document.body.appendChild(notification);

        // Animate in
        setTimeout(() => {
            notification.style.transform = 'translateX(0)';
        }, 100);

        // Auto remove
        setTimeout(() => {
            notification.style.transform = 'translateX(400px)';
            setTimeout(() => {
                if (notification.parentNode) {
                    notification.parentNode.removeChild(notification);
                }
            }, 300);
        }, 3000);
    }
}

// Initialize app when DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
    window.app = new FinanceApp();
});

// Close modal when clicking outside
document.addEventListener('click', (e) => {
    if (e.target.classList.contains('modal')) {
        e.target.classList.remove('active');
    }
});
