disp("Welcome to Shawn Malinao's Bisection Method Calculator where it finds the root of an equation with the said Method");
% Coefficients and Terms
user_prompt_for_coefficient_value = "Value for the coefficient of";
Coefficient_list = [Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th];
Coefficient_1st = input(user_prompt_for_coefficient_value + " x^4 ");
Coefficient_2nd = input(user_prompt_for_coefficient_value + " x^3 ");
Coefficient_3rd = input(user_prompt_for_coefficient_value + " x^2 ");
Coefficient_4th = input(user_prompt_for_coefficient_value + " x ");
Coefficient_5th = input(user_prompt_for_coefficient_value + " Constant ");
% Initialize arrays
function [a_values, b_values, x_values, y_values, sign_checker, loop_flag, counter, Status_values, Iteration_values, Table, Continue_user, Continue_user_validation, Iteration] = initialize_arrays()
a_values = [];
b_values = [];
x_values = [];
y_values = [];
sign_checker = 0;
loop_flag = true;
counter = 0;
Status_values = [];
Iteration_values = [];
Table = [];
Continue_user = "Y";
Continue_user_validation = 0;
Iteration = 1;
end
[a_values, b_values, x_values, y_values, sign_checker, loop_flag, counter, Status_values, Iteration_values, Table, Continue_user, Continue_user_validation, Iteration] = initialize_arrays();

%functions
% Inputting a, b, and x and calculating  the Terms
%   1) User Inputs a and b
function [a, b] = user_prompt_a_b ()
a = double(input("Initial value for a   "));
b = double(input("Initial value for b   "));
end
function [x] = calculate_iterating_x (a, b)
x = (a + b) / 2;
end
%%   1) Calculates the coefficient
function [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th)
Term_1st = Coefficient_1st*x^4;
Term_2nd = Coefficient_2nd*x^3;
Term_3rd = Coefficient_3rd*x^2;
Term_4th = Coefficient_4th*x;
Term_5th = Coefficient_5th;
end
%%   2) valculate y
function [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th)
y = Term_1st + Term_2nd + Term_3rd + Term_4th + Term_5th;
end
%%   3) sign_checker
function [sign_checker] = sign_checker_detector (y , y_prev)
sign_checker = y * y_prev;
end
%%   4) Update arrays
function [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y)
a_values = [a_values; a];
b_values = [b_values; b];
x_values = [x_values; x];
y_values = [y_values; y];
end
%%   5) Update Iteration and a or b value
function [Iteration , Iteration_values, c] = Update_Iteration (Iteration_values, Iteration, c, d)
Iteration = Iteration + 1;
Iteration_values = [Iteration_values; Iteration];
c = c + d;
end
%%   6) Removes the displaced value of x
function [iteration_start_a, a, sign_change_detector] = prepare_while_loop(a, Displacement)
% Function body: Prepares the iteration in while loops
iteration_start_a = a - Displacement;
a = iteration_start_a;
sign_change_detector = 1;
end
%% 9) Loop Back
function [Continue_user, counter, sign_change_detector] = Loop_Back ()
Continue_user = "";
counter = 0;
sign_change_detector = 0;
loop_flag = true;
end
%%  8) To Loop Back?
function [Continue_user, Continue_user_validation] = User_prompt_Loop_back (Continue_user)
if Continue_user == "Y"
    Continue_user = "Y";
    Continue_user_validation = 1;
elseif Continue_user == "N"
    Continue_user = "N";
    Continue_user_validation = 1;
    return
else
    Continue_user = input("Would you like to Continue? Y/N", 's');
    Continue_user_validation = 0;
end
end


% Bisection Method
while Continue_user == "Y"
    [Continue_user, counter, sign_change_detector] = Loop_Back ();
    %% Initial
    Iteration_values = [Iteration_values; Iteration];
    [a, b] = user_prompt_a_b ();
    [x] = calculate_iterating_x (a, b);
    [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
    [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
    y_prev = y;
    [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
    %% Whole Number
    while loop_flag == true
        counter = counter + 1;
        [Iteration , Iteration_values, a] = Update_Iteration (Iteration_values, Iteration, a, 1);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
        [Iteration , Iteration_values, b] = Update_Iteration (Iteration_values, Iteration, b, 1);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
        if counter > 50
            disp(table(Iteration_values , a_values, b_values, x_values, y_values));
            disp("You reached the maximum amount of Iterations. The Inputted x is either far from the root or there is no root following the input x")
            return
        else
            continue
        end
    end
    %% Tenths Place 1dd
    [~, a, ~] = prepare_while_loop(a, 1);
    while loop_flag == true
        counter = counter + 1;
        [Iteration, Iteration_values, a] = Update_Iteration (Iteration_values, Iteration, a, 0.1);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
        [Iteration, Iteration_values, b] = Update_Iteration (Iteration_values, Iteration, b, 0.1);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
    end
    %% Hundreths Place 2dd
    [~, a, ~] = prepare_while_loop(a, 0.1);
    while loop_flag == true
        counter = counter + 1;
        [Iteration, Iteration_values, a] = Update_Iteration (Iteration_values, Iteration, a, 0.01);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
        [Iteration, Iteration_values, b] = Update_Iteration (Iteration_values, Iteration, b, 0.01);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
    end
    %% Thousandths Place 3dd
    [~, a, ~] = prepare_while_loop(a, 0.01);
    while loop_flag == true
        counter = counter + 1;
        [Iteration, Iteration_values, a] = Update_Iteration (Iteration_values, Iteration, a, 0.001);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
        [Iteration, Iteration_values, b] = Update_Iteration (Iteration_values, Iteration, b, 0.001);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
    end
    %% Ten Thousandths Place 4dd
    [~, a, ~] = prepare_while_loop(a, 0.001);
    while loop_flag == true
        counter = counter + 1;
        [Iteration, Iteration_values, a] = Update_Iteration (Iteration_values, Iteration, a, 0.0001);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
        [Iteration, Iteration_values, b] = Update_Iteration (Iteration_values, Iteration, b, 0.0001);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
    end
    %% Hundred Thousandths Place 5dd
    [~, a, ~] = prepare_while_loop(a, 0.0001);
    while loop_flag == true
        counter = counter + 1;
        [Iteration, Iteration_values, a] = Update_Iteration (Iteration_values, Iteration, a, 0.00001);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
        [Iteration, Iteration_values, b] = Update_Iteration (Iteration_values, Iteration, b, 0.00001);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
    end
    %% Millionts Place 5dd
    [iteration_start_a, a, sign_change_detector] = prepare_while_loop(a, 0.00001);
    while loop_flag == true
        counter = counter + 1;
        [Iteration, Iteration_values, a] = Update_Iteration (Iteration_values, Iteration, a, 0.000001);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
        [Iteration, Iteration_values, b] = Update_Iteration (Iteration_values, Iteration, b, 0.000001);
        [x] = calculate_iterating_x (a, b);
        [Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th] = update_coefficients(x, Coefficient_1st, Coefficient_2nd, Coefficient_3rd, Coefficient_4th, Coefficient_5th);
        [y] = Calculate_y (Term_1st, Term_2nd, Term_3rd, Term_4th, Term_5th);
        [a_values, b_values, x_values, y_values] = update_arrays(a_values, a, b_values, b, x_values, x, y_values, y);
        [sign_checker] = sign_checker_detector (y , y_prev);
        if sign_checker <= 0
            break
        end
    end
    
    %Summarizes the Iteration in a table
    Table = table(Iteration_values);
    % Evaluates each x to create conclusion
    for i = 1:size(Table, 1)
        x_value_formatted = sprintf('%.6f', x_values(i));
        if abs(y_values(i)) <= 0.0000005
            Conclusion = "  " + x_value_formatted + " is the root of the system  ";
        else
            Conclusion = x_value_formatted + " is not the root of the system";
        end
        Conclusion_values{i} = Conclusion;
    end
    % Adds columns to table
    headings = {'a', 'b', 'x', 'f((a+b)/2)', 'Conclusion'};
    value_arrays = {a_values, b_values, x_values, y_values, Conclusion_values'};
    for i = 1:numel(headings)
        Table.(headings{i}) = value_arrays{i};
    end
    % Renames the headings of the table
    Table.Properties.VariableNames = {'Number of Iteration', 'a', 'b', '(a+b)/2', 'f((a+b)/2)', 'Conclusion'};
    % Specifies the decimals places
    Decimals_To_Be_Specified = {'a', 'b', '(a+b)/2', 'f((a+b)/2)'};
    for i = 1:numel(Decimals_To_Be_Specified)
        column_name = Decimals_To_Be_Specified{i};
        Table.(column_name) = num2str(Table.(column_name), '%.6f');
    end
    disp(Table)
    disp("Made by: Shawn Michael M. Malinao BSME 2")

    Continue_user = input("Would you like to Continue? Y/N", 's');
    while Continue_user_validation == 0
        [Continue_user, Continue_user_validation] = User_prompt_Loop_back (Continue_user);
    end
end
