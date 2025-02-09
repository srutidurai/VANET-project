% Simulation of VANET with Conditional Privacy Protection Scheme

% Number of vehicles
num_vehicles = 10;

% Vehicle positions (in a 2D grid, for example, on a highway or a city grid)
vehicle_positions = rand(num_vehicles, 2) * 100; % random positions in a 100x100 area

% Communication Range
comm_range = 20; % Vehicles can communicate with others within this range

% Conditional Privacy: Vehicles can only reveal their position if within communication range
privacy_enabled = true;  % Flag for conditional privacy

% Message structure (each vehicle can send a message to others)
messages = cell(num_vehicles, 1);
for i = 1:num_vehicles
    messages{i} = sprintf('Vehicle %d: Message Content', i);
end

% Simulation loop (each vehicle will try to communicate with others)
for t = 1:100 % Simulate for 100 time steps
    
    % Loop through each vehicle to send messages to nearby vehicles
    for i = 1:num_vehicles
        for j = 1:num_vehicles
            if i ~= j
                % Calculate the distance between vehicle i and vehicle j
                dist = norm(vehicle_positions(i,:) - vehicle_positions(j,:));
                
                % Conditional Privacy Protection: Only communicate if within range
                if dist <= comm_range
                    if privacy_enabled
                        fprintf('Vehicle %d sends message to Vehicle %d: "%s" at time %d\n', ...
                            i, j, messages{i}, t);
                    else
                        fprintf('Vehicle %d sends message to Vehicle %d: "%s" (no privacy) at time %d\n', ...
                            i, j, messages{i}, t);
                    end
                else
                    % Vehicle j is out of range, no communication
                    fprintf('Vehicle %d cannot communicate with Vehicle %d due to distance > %d at time %d\n', ...
                            i, j, comm_range, t);
                end
            end
        end
    end
    
    % Update vehicle positions for the next time step (simple random movement)
    vehicle_positions = vehicle_positions + (rand(num_vehicles, 2) - 0.5) * 2; % random movement
end

% Plot vehicle positions at the end of the simulation
figure;
scatter(vehicle_positions(:,1), vehicle_positions(:,2), 'filled');
title('Vehicle Positions in VANET');
xlabel('X Position');
ylabel('Y Position');
grid on;
