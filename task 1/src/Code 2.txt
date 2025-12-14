% ===============================================================
% Biotransport Assignment 01 - Analytical Functions (Backend)
% ===============================================================
% Contains all computational logic for Re calculation, tables, and plotting.
% Fully implements Part 1, Part 2, Part 3, and Part 5 requirements.

function varargout = biotransport_analysis_functions(func_name, varargin)
    if nargin == 0
        disp('Error: This is a backend function file and should be called by the GUI.');
        return;
    end
    [varargout{1:nargout}] = feval(func_name, varargin{:});
end

%% ===========================
% --- PART 1: Reynolds Number Calculator & Regime Classification ---
%% ===========================
function [Results, LastCaseData] = RunCaseAnalysis(~, ~)
    % Define fluid properties
    fluid_map = struct(...
        'Water_20C', struct('rho', 998.2,  'mu', 1.002e-3), ...
        'Water_37C', struct('rho', 993.3,  'mu', 0.691e-3), ...
        'Water_50C', struct('rho', 988.1,  'mu', 0.547e-3), ...
        'Air_25C',   struct('rho', 1.184,  'mu', 1.849e-5), ...
        'Blood',     struct('rho', 1060,   'mu', 3.5e-3), ...
        'CSF',       struct('rho', 1005,   'mu', 0.7e-3) ...
    );

    % Geometry sets
    circular_D = [0.010, 0.025, 0.050, 0.100];
    square_side = [0.010, 0.025, 0.050];
    rect_WH = [0.080, 0.040; 0.050, 0.020; 0.025, 0.010];
    annulus_DoDi = [0.100, 0.050; 0.050, 0.020];

    velocities = [0.10, 0.50, 1.00, 2.00];
    fluid_keys = fieldnames(fluid_map);

    case_rows = {};

    for fidx = 1:numel(fluid_keys)
        fkey = fluid_keys{fidx};
        rho = fluid_map.(fkey).rho;
        mu  = fluid_map.(fkey).mu;

        % Circular
        for D = circular_D
            Dh = D;
            for v = velocities
                Re = (rho * v * Dh) / mu;
                regime = classifyRe(Re);
                f = computeDarcyF(Dh, Re, regime, 'Circular Pipe');
                case_rows(end+1,:) = {strrep(fkey,'_',' '), ...
                    sprintf('Circular (D=%.3f m)', D), sprintf('D=%.3f', D), v, Dh, Re, regime, f};
            end
        end

        % Square
        for s = square_side
            Dh = s;
            for v = velocities
                Re = (rho * v * Dh) / mu;
                regime = classifyRe(Re);
                f = computeDarcyF(Dh, Re, regime, 'Square Duct');
                case_rows(end+1,:) = {strrep(fkey,'_',' '), ...
                    sprintf('Square (side=%.3f m)', s), sprintf('side=%.3f', s), v, Dh, Re, regime, f};
            end
        end

        % Rectangular
        for r = 1:size(rect_WH,1)
            W = rect_WH(r,1);
            H = rect_WH(r,2);
            Dh = (2*W*H)/(W+H);
            for v = velocities
                Re = (rho * v * Dh) / mu;
                regime = classifyRe(Re);
                f = computeDarcyF(Dh, Re, regime, 'Rectangular Duct');
                case_rows(end+1,:) = {strrep(fkey,'_',' '), ...
                    sprintf('Rectangular (W=%.3f,H=%.3f)', W, H), sprintf('W=%.3f, H=%.3f', W, H), v, Dh, Re, regime, f};
            end
        end

        % Annulus
        for a = 1:size(annulus_DoDi,1)
            Do = annulus_DoDi(a,1);
            Di = annulus_DoDi(a,2);
            if Di >= Do, continue; end
            Dh = Do - Di;
            for v = velocities
                Re = (rho * v * Dh) / mu;
                regime = classifyRe(Re);
                f = computeDarcyF(Dh, Re, regime, 'Concentric Annulus');
                case_rows(end+1,:) = {strrep(fkey,'_',' '), ...
                    sprintf('Annulus (Do=%.3f, Di=%.3f)', Do, Di), sprintf('Do=%.3f, Di=%.3f', Do, Di), v, Dh, Re, regime, f};
            end
        end
    end

    Results = cell2table(case_rows, 'VariableNames', ...
        {'FluidName','Configuration','Dimensions_m','Velocity_ms','HydraulicDiameter_m','ReynoldsNumber','FlowRegime','DarcyFrictionFactor'});

    % Last case data
    lastIdx = height(Results);
    LastCaseData = struct( ...
        'D', Results.HydraulicDiameter_m(lastIdx), ...
        'v_avg', Results.Velocity_ms(lastIdx), ...
        'Re', Results.ReynoldsNumber(lastIdx), ...
        'regime', Results.FlowRegime{lastIdx}, ...
        'name', [Results.FluidName{lastIdx}, ' - ', Results.Configuration{lastIdx}] ...
    );
end

% ===== Helper Functions =====
function regime = classifyRe(Re)
    if Re < 2000
        regime = 'Laminar';
    elseif Re < 4000
        regime = 'Transition';
    else
        regime = 'Turbulent';
    end
end

function f = computeDarcyF(~, Re, regime, shapeType)
    if Re <= 0
        f = NaN; return;
    end
    switch lower(regime)
        case 'laminar'
            switch shapeType
                case 'Circular Pipe'
                    f = 64/Re;
                case 'Square Duct'
                    f = 56.92/Re;
                otherwise
                    f = 64/Re;
            end
        case 'turbulent'
            f = 0.3164/(Re^0.25);
        otherwise
            f = NaN;
    end
end

%% ===========================
% --- PART 2: Flow Profile Visualization ---
%% ===========================
function PlotFlowProfiles(ax, D, v_avg, Re, regime, case_name)
    cla(ax);
    if nargin < 6, case_name = ''; end
    if isempty(D) || D<=0
        title(ax,'Invalid inputs','Color',[0.6 0 0]); return;
    end

    R = D/2;
    r_abs = linspace(0,R,100);
    r_pos = linspace(-R,R,199);

    v_max_laminar = 2*v_avg;
    u_laminar_half = v_max_laminar*(1-(r_abs./R).^2);
    u_laminar_full = [fliplr(u_laminar_half(1:end-1)), u_laminar_half];

    v_max_turbulent = v_avg/0.817;
    u_turbulent_half = v_max_turbulent*(1-(r_abs./R)).^(1/7);
    u_turbulent_full = [fliplr(u_turbulent_half(1:end-1)), u_turbulent_half];

    hold(ax,'on');
    if strcmpi(regime,'Laminar') || strcmpi(regime,'Transition')
        plot(ax,r_pos,u_laminar_full,'LineWidth',3,'DisplayName','Laminar');
        plot(ax,r_pos,u_turbulent_full,'--','LineWidth',1.5,'DisplayName','Turbulent');
    else
        plot(ax,r_pos, u_turbulent_full,'LineWidth',3,'DisplayName','Turbulent');
        plot(ax,r_pos, u_laminar_full,'--','LineWidth',1.5,'DisplayName','Laminar');
    end

    xline(ax,v_avg,'k:','LineWidth',1,'DisplayName','Average Velocity');
    title(ax,sprintf('Velocity Profiles (Re=%.1f, %s) %s',Re,regime,case_name));
    ylabel(ax,'Axial Velocity (m/s)');
    xlabel(ax,'Radial Position (m)');
    legend(ax,'Location','best');
    grid(ax,'on'); hold(ax,'off');
end

%% ===========================
% --- PART 3: Friction Factor Plot ---
%% ===========================
function PlotFrictionFactor(ax)
    cla(ax);
    Re_range = logspace(1, 6, 200);
    f_laminar = 64 ./ Re_range;
    loglog(ax,Re_range(Re_range<=2000),f_laminar(Re_range<=2000),'b-','LineWidth',2,'DisplayName','f = 64/Re (Laminar)');
    hold(ax,'on');
    
    epsilon_concrete_coarse = 0.25;
    epsilon_concrete_smooth = 0.025;
    epsilon_drawn_tubing = 0.0025;
    epsilon_glass_plastic_perspex = 0.0025;
    epsilon_iron_cast = 0.15;
    epsilon_sewers_old = 3.0;
    epsilon_steel_mortar_lined = 0.1;
    epsilon_steel_rusted = 0.5;
    epsilon_steel_structural_forged = 0.025;
    epsilon_water_mains_old = 1.0;
    epsilon_blood_vessel = 1500.0;

    f_turbulent_coarse = 0.25 ./ (log10((epsilon_concrete_coarse/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_smooth = 0.25 ./ (log10((epsilon_concrete_smooth/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_tubing = 0.25 ./ (log10((epsilon_drawn_tubing/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_plastic = 0.25 ./ (log10((epsilon_glass_plastic_perspex/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_iron = 0.25 ./ (log10((epsilon_iron_cast/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_sewers = 0.25 ./ (log10((epsilon_sewers_old/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_steel_mortar = 0.25 ./ (log10((epsilon_steel_mortar_lined/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_steel_rusted = 0.25 ./ (log10((epsilon_steel_rusted/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_steel_forged = 0.25 ./ (log10((epsilon_steel_structural_forged/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_water_mains = 0.25 ./ (log10((epsilon_water_mains_old/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    f_turbulent_blood_vessel = 0.25 ./ (log10((epsilon_blood_vessel/3.7) + (5.74 ./ (Re_range.^0.9)))).^2;
    
    % Plot each material (turbulent region: Re >= 4000)
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_coarse(Re_range >= 4000), 'r--', 'LineWidth', 2, 'DisplayName', 'Concrete, coarse');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_smooth(Re_range >= 4000), 'b--', 'LineWidth', 2, 'DisplayName', 'Concrete, new smooth');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_tubing(Re_range >= 4000), 'g--', 'LineWidth', 2, 'DisplayName', 'Drawn tubing');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_plastic(Re_range >= 4000), 'c--', 'LineWidth', 2, 'DisplayName', 'Glass/Plastic/Perspex');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_iron(Re_range >= 4000), 'm--', 'LineWidth', 2, 'DisplayName', 'Iron, cast');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_sewers(Re_range >= 4000), 'y--', 'LineWidth', 2, 'DisplayName', 'Sewers, old');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_steel_mortar(Re_range >= 4000), 'k--', 'LineWidth', 2, 'DisplayName', 'Steel, mortar lined');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_steel_rusted(Re_range >= 4000), 'y-', 'LineWidth', 2, 'DisplayName', 'Steel, rusted');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_steel_forged(Re_range >= 4000), 'b.', 'LineWidth', 2, 'DisplayName', 'Steel, structural/forged');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_water_mains(Re_range >= 4000), 'g-', 'LineWidth', 2, 'DisplayName', 'Water mains, old');
    loglog(ax, Re_range(Re_range >= 4000), f_turbulent_blood_vessel(Re_range >= 4000), 'r-.', 'LineWidth', 2, 'DisplayName', 'Blood Vessel');


    xline(ax,2000,'g:','LineWidth',1.5,'DisplayName','Laminar → Transition');
    xline(ax,4000,'m:','LineWidth',1.5,'DisplayName','Turbulent Start');
    title(ax,'Darcy Friction Factor vs Reynolds Number');
    xlabel(ax,'Reynolds Number (Re)');
    ylabel(ax,'Friction Factor (f)');
    legend(ax,'Location','southwest');
    grid(ax,'on'); axis(ax,[10 1e6 0.005 0.1]);
    hold(ax,'off');
end

%% ===========================
% --- PART 3 (Extra): Multiple Velocity Profiles for different Re ---
%% ===========================
function PlotMultipleProfiles(ax, D)
    cla(ax);
    R = D/2;
    r_abs = linspace(0,R,100);
    Re_values = [500 5000 50000];
    v_avg = 1; % fixed average velocity
    colors = lines(numel(Re_values));

    hold(ax,'on');
    for i = 1:numel(Re_values)
        Re = Re_values(i);
        if Re < 2000
            v_max = 2*v_avg;
            u = v_max*(1-(r_abs./R).^2);
        else
            v_max = v_avg/0.817;
            u = v_max*(1-(r_abs./R)).^(1/7);
        end
        plot(ax,[fliplr(u(1:end-1)),u],linspace(-R,R,199),...
            'LineWidth',2,'Color',colors(i,:),'DisplayName',['Re = ',num2str(Re)]);
    end
    xline(ax,v_avg,'k:','DisplayName','Average Velocity');
    title(ax,'Velocity Profiles for Different Reynolds Numbers');
    xlabel(ax,'Axial Velocity (m/s)');
    ylabel(ax,'Radial Position (m)');
    legend(ax,'Location','best');
    grid(ax,'on');
    hold(ax,'off');
end

%% ===========================
% --- PART 5: Conceptual Flow Visualization ---
%% ===========================
function PlotConceptualFlow_Final(ax_laminar, ax_transition, ax_turbulent)
    [X,Y] = meshgrid(0:0.05:1,0:0.05:1);

    % Laminar
    cla(ax_laminar);
    U = ones(size(X)); V = zeros(size(Y));
    quiver(ax_laminar,X,Y,U,V,0.8);
    streamline(ax_laminar,X,Y,U,V,X(1,:),Y(1,:));
    axis(ax_laminar,[0 1 0 1]);
    title(ax_laminar,'Laminar (Smooth Flow)');
    xlabel(ax_laminar,'X'); ylabel(ax_laminar,'Y');

    % Transition
    cla(ax_transition);
    U = 1+0.25*sin(Y*8).*cos(X*5); V = 0.2*sin(X*7);
    quiver(ax_transition,X,Y,U,V,0.8);
    axis(ax_transition,[0 1 0 1]);
    title(ax_transition,'Transition (Wavy)');

    % Turbulent
    cla(ax_turbulent);
    rng(2);
    U = 1+0.6*randn(size(X)); V = 0.6*randn(size(Y));
    quiver(ax_turbulent,X,Y,U,V,0.8);
    axis(ax_turbulent,[0 1 0 1]);
    title(ax_turbulent,'Turbulent (Chaotic)');
end
