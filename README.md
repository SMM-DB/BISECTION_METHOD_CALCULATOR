disp("Welcome to Shawn Malinao's Adjoint Method Calculator where it calculates the solutions with said method");

%%  1) Loop Back
function [Continue_user, Continue_user_validation] = Loop_Back ()
Continue_user = "";
Continue_user_validation = 1;
end
%%  2) To Loop Back?
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

A = [];
Adjoint_of_A = [];
Inverse_of_A = [];
Inverse_of_A_not_6_decimals = [];
Continue_user = "Y";
Continue_user_validation = 1;

while Continue_user == "Y"
    [Continue_user, Continue_user_validation] = Loop_Back ();
    
    size_of_matrix = input('Enter the size of the square matrix: ');
    A = zeros(size_of_matrix);
    disp('Enter elements of the matrix:');
    for i = 1:size_of_matrix
        for j = 1:size_of_matrix
            prompt = sprintf('Enter element A(%d,%d): ', i, j);
            A(i,j) = input(prompt);
        end
    end
    disp("Here's Matrix A ");
    disp(A);

    B = zeros(j, 1);
    for i = 1:j
        B(i) = input(['Enter the value for element in row ', num2str(i), ': ']);
    end

    disp("Here's your system of equations in matrix form")
    disp ([A, B]);

    % This is where the Matrix A turned into its Inverse Counterpart
    Adjoint_of_A = adjoint(A);
    determinant_of_A = det(A);
    Inverse_of_A = Adjoint_of_A / determinant_of_A;

    Solved_Variables_not_6_decimals = Inverse_of_A * B;

    format long;
    Solved_Variables = round(Solved_Variables_not_6_decimals, 6);

    disp('Value of each variables:');
    fprintf('%0.6f %0.6f\n', Solved_Variables');
    disp("Made by: Shawn Michael M. Malinao BSME 2")

    while Continue_user_validation == 1
        [Continue_user, Continue_user_validation] = User_prompt_Loop_back (Continue_user);
    end
end
