map_html = '''
{% extends "base.html" %}
{% block title %}Interactive Map - African Critical Minerals Tracker{% endblock %}
{% block content %}
<div class="d-flex justify-content-between align-items-center mb-4">
    <h1><i class="fas fa-map-marked-alt me-2"></i>Interactive Minerals Map</h1>
    <a href="{{ url_for('minerals_map') }}" class="btn btn-outline-primary">
        <i class="fas fa-sync-alt me-1"></i>Refresh Map
    </a>
</div>

<p class="lead mb-4">Explore mining sites and mineral deposits across Africa. Click on markers for detailed information.</p>

<div class="card feature-card">
    <div class="card-body p-0">
        <div style="height: 70vh; width: 100%;">
            <iframe src="{{ url_for('static', filename='maps/minerals_map.html') }}" 
                    width="100%" height="100%" frameborder="0" style="border-radius: 8px;"></iframe>
        </div>
    </div>
</div>

<div class="row mt-4">
    <div class="col-md-12">
        <div class="card">
            <div class="card-header">
                <h5 class="mb-0"><i class="fas fa-info-circle me-2"></i>Map Legend</h5>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col-md-2 text-center">
                        <i class="fas fa-map-marker-alt text-primary"></i> Cobalt
                    </div>
                    <div class="col-md-2 text-center">
                        <i class="fas fa-map-marker-alt text-success"></i> Lithium
                    </div>
                    <div class="col-md-2 text-center">
                        <i class="fas fa-map-marker-alt text-dark"></i> Graphite
                    </div>
                    <div class="col-md-2 text-center">
                        <i class="fas fa-map-marker-alt text-warning"></i> Manganese
                    </div>
                    <div class="col-md-2 text-center">
                        <i class="fas fa-map-marker-alt text-info"></i> Platinum
                    </div>
                    <div class="col-md-2 text-center">
                        <i class="fas fa-map-marker-alt text-danger"></i> Copper
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<div class="row mt-4">
    <div class="col-12">
        <div class="card feature-card">
            <div class="card-body text-center">
                <a href="{{ url_for('source_code') }}" class="btn btn-outline-secondary" target="_blank">
                    <i class="fas fa-code me-1"></i>View Application Source Code
                </a>
            </div>
        </div>
    </div>
</div>
{% endblock %}
'''

charts_html = '''
{% extends "base.html" %}
{% block title %}Analytics - African Critical Minerals Tracker{% endblock %}
{% block content %}
<div class="d-flex justify-content-between align-items-center mb-4">
    <h1><i class="fas fa-chart-bar me-2"></i>Data Analytics</h1>
    <span class="badge bg-info fs-6">Interactive Charts</span>
</div>

<p class="lead mb-4">Visual analysis of mineral production and export trends across African countries.</p>

<div class="row">
    <div class="col-md-12 mb-4">
        <div class="card feature-card">
            <div class="card-header">
                <h5 class="mb-0"><i class="fas fa-chart-bar me-2"></i>Production by Country</h5>
            </div>
            <div class="card-body">
                <div id="chart1"></div>
            </div>
        </div>
    </div>
</div>

<div class="row">
    <div class="col-md-12">
        <div class="card feature-card">
            <div class="card-header">
                <h5 class="mb-0"><i class="fas fa-chart-pie me-2"></i>Export Value by Mineral</h5>
            </div>
            <div class="card-body">
                <div id="chart2"></div>
            </div>
        </div>
    </div>
</div>

<div class="row mt-4">
    <div class="col-12">
        <div class="card feature-card">
            <div class="card-body text-center">
                <a href="{{ url_for('source_code') }}" class="btn btn-outline-secondary" target="_blank">
                    <i class="fas fa-code me-1"></i>View Application Source Code
                </a>
            </div>
        </div>
    </div>
</div>
{% endblock %}

{% block scripts %}
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
    var graph1 = {{ graph1JSON | safe }};
    Plotly.newPlot('chart1', graph1.data, graph1.layout);
    
    var graph2 = {{ graph2JSON | safe }};
    Plotly.newPlot('chart2', graph2.data, graph2.layout);
</script>
{% endblock %}
'''

admin_users_html = '''
{% extends "base.html" %}
{% block title %}User Management - African Critical Minerals Tracker{% endblock %}
{% block content %}
<div class="d-flex justify-content-between align-items-center mb-4">
    <h1><i class="fas fa-users-cog me-2"></i>User Management</h1>
    <span class="badge bg-warning fs-6">{{ users|length }} Users</span>
</div>

<div class="card feature-card">
    <div class="card-header">
        <h5 class="mb-0"><i class="fas fa-user-shield me-2"></i>System Users</h5>
    </div>
    <div class="card-body">
        <div class="table-responsive">
            <table class="table table-striped">
                <thead class="table-dark">
                    <tr>
                        <th>Username</th>
                        <th>Role</th>
                        <th>Email</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody>
                    {% for user in users %}
                    <tr>
                        <td>
                            <i class="fas fa-user me-2"></i>{{ user.Username }}
                            {% if user.RoleName == 'Administrator' %}
                            <span class="badge bg-danger ms-2">Admin</span>
                            {% endif %}
                        </td>
                        <td>{{ user.RoleName }}</td>
                        <td>{{ user.Email }}</td>
                        <td>
                            <form method="POST" action="{{ url_for('admin_reset_password', username=user.Username) }}" 
                                  style="display: inline;" onsubmit="return confirm('Reset password for {{ user.Username }}?')">
                                <button type="submit" class="btn btn-warning btn-sm">
                                    <i class="fas fa-key me-1"></i>Reset Password
                                </button>
                            </form>
                        </td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>
        </div>
    </div>
</div>

<div class="row mt-4">
    <div class="col-12">
        <div class="card feature-card">
            <div class="card-body text-center">
                <a href="{{ url_for('source_code') }}" class="btn btn-outline-secondary" target="_blank">
                    <i class="fas fa-code me-1"></i>View Application Source Code
                </a>
            </div>
        </div>
    </div>
</div>
{% endblock %}
'''

source_code_html = '''
{% extends "base.html" %}
{% block title %}Source Code - African Critical Minerals Tracker{% endblock %}
{% block content %}
<div class="row">
    <div class="col-12">
        <div class="card feature-card">
            <div class="card-header bg-dark text-white">
                <h4 class="mb-0"><i class="fas fa-code me-2"></i>Application Source Code</h4>
            </div>
            <div class="card-body">
                <div class="alert alert-info">
                    <h5><i class="fas fa-info-circle me-2"></i>About this Application</h5>
                    <p class="mb-2"><strong>African Critical Minerals Tracker</strong> - A comprehensive web application for tracking and analyzing critical mineral resources across Africa.</p>
                    <p class="mb-0"><strong>University:</strong> University of the Witwatersrand | <strong>Course:</strong> MINN2020A: Computer Programming for Mining</p>
                </div>
                
                <h5 class="mt-4"><i class="fas fa-file-code me-2"></i>Complete Python Code</h5>
                <div class="bg-light p-3 rounded border" style="max-height: 600px; overflow-y: auto;">
                    <pre><code class="language-python">{{ source_code }}</code></pre>
                </div>
                
                <div class="mt-4">
                    <h5><i class="fas fa-download me-2"></i>Quick Actions</h5>
                    <div class="row">
                        <div class="col-md-4">
                            <a href="javascript:void(0);" onclick="copyToClipboard()" class="btn btn-outline-primary w-100 mb-2">
                                <i class="fas fa-copy me-1"></i>Copy Code
                            </a>
                        </div>
                        <div class="col-md-4">
                            <a href="{{ url_for('download_code') }}" class="btn btn-outline-success w-100 mb-2">
                                <i class="fas fa-download me-1"></i>Download Python File
                            </a>
                        </div>
                        <div class="col-md-4">
                            <a href="{{ url_for('dashboard') }}" class="btn btn-outline-secondary w-100 mb-2">
                                <i class="fas fa-arrow-left me-1"></i>Back to App
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}

{% block scripts %}
<script>
function copyToClipboard() {
    const codeText = `{{ source_code }}`;
    navigator.clipboard.writeText(codeText).then(function() {
        // Show success message
        const originalText = event.target.innerHTML;
        event.target.innerHTML = '<i class="fas fa-check me-1"></i>Copied!';
        event.target.classList.remove('btn-outline-primary');
        event.target.classList.add('btn-success');
        
        setTimeout(function() {
            event.target.innerHTML = originalText;
            event.target.classList.remove('btn-success');
            event.target.classList.add('btn-outline-primary');
        }, 2000);
    }).catch(function(err) {
        console.error('Failed to copy: ', err);
        alert('Failed to copy code to clipboard');
    });
}
</script>
{% endblock %}
'''# MINN-2020A-CODE
