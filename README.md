# GaussianEliminationGUi
#include <iostream>
#include <vector>
#include <iomanip> // For setting precision
#include <string>
#ifdef _WIN32
#include <windows.h> // For GUI on Windows
#else
#error GUI implementation is only supported on Windows for this version.
#endif

using namespace std;

// Function for Gaussian Elimination
void gaussian_Elimination(vector<vector<double>>& matrix, int n) 
{
    for (int i = 0; i < n; i++) 
    {
        // Pivoting
        if (matrix[i][i] == 0) 
        {
            for (int j = i + 1; j < n; j++) 
            {
                if (matrix[j][i] != 0)
                {
                    swap(matrix[i], matrix[j]);
                    break;
                }
            }
        }

        double pivot = matrix[i][i];
        if (pivot != 0)
        {
            for (int j = 0; j <= n; j++)
            {
                matrix[i][j] /= pivot;
            }
        }

        for (int j = 0; j < n; j++) 
        {
            if (j != i)
            {
                double factor = matrix[j][i];
                for (int k = 0; k <= n; k++)
                {
                    matrix[j][k] -= factor * matrix[i][k];
                }
            }
        }
    }
}

// Function to print the solution
void print_Solution(const vector<vector<double>>& matrix, int n) 
{
    cout << "Solution:" << endl;
    for (int i = 0; i < n; i++) 
    {
        cout << "x" << i + 1 << " = " << fixed << setprecision(4) << matrix[i][n] << endl;
    } 
}

// Function to test root correctness
void testRoots(const vector<vector<double>>& main_Matrix, const vector<vector<double>>& matrix, int n) 
{
    cout << "\nTesting the roots of the matrix:" << endl;
    for (int i = 0; i < n; i++) 
    {
        double l = 0;
        for (int j = 0; j < n; j++) 
        {
            l += main_Matrix[i][j] * matrix[j][n];
        }
        cout << "Equation " << i + 1 << ": " << l << " = " << main_Matrix[i][n] << " -> ";
        if (abs(l - main_Matrix[i][n]) < 1e-6)
        {
            cout << "Correct" << endl;
        }
        else
        {
            cout << "Incorrect" << endl;
        }
    }
}

// GUI Functionality (Simple Windows Message Box for Demonstration)
void show_GUI(vector<vector<double>>& matrix, int n)
{
    string result = "Gaussian Elimination Results:\n";

    for (int i = 0; i < n; i++)
    {
        result += "x" + to_string(i + 1) + " = " + to_string(matrix[i][n]) + "\n";
    }

    MessageBox(NULL, result.c_str(), "Solution", MB_OK);
}

int main() 
{
    int n; 

    // Asking the user for the number of variables
    cout << "Enter the number of variables: ";
    cin >> n;

    vector<vector<double>> matrix(n, vector<double>(n + 1));
    vector<vector<double>> main_Matrix(n, vector<double>(n + 1));

    // Input augmented matrix
    cout << "Enter the augmented matrix:" << endl;
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j <= n; j++) 
        {
            cin >> matrix[i][j];
            main_Matrix[i][j] = matrix[i][j];
        }
    }

    // Perform Gaussian Elimination
    gaussian_Elimination(matrix, n);

    // Print the solution
    print_Solution(matrix, n);

    // Test root correctness
    testRoots(main_Matrix, matrix, n);

    // Show GUI with results
    show_GUI(matrix, n);

    return 0;
}
