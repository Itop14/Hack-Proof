# Hack-Proof C#

Create a program that will only open a text document if the correct password is entered. The user should choose the username and password first and it should also verify
the password before allowing it.
Extensions:
Create a random password first of at least 8 characters first as a suggested password
Create a random password that contains at least a lowercase, uppercase and special character of at least 8 characters in length



      using System;
      using System.Collections.Generic;
      using System.ComponentModel;
      using System.Data;
      using System.Drawing;
      using System.Linq;
      using System.Text;
      using System.Threading.Tasks;
      using System.Windows.Forms;
      using System.Diagnostics;
      using System.Text.RegularExpressions;
      
      namespace Hack_Proof
      {
          public partial class LoginPage : Form
          {
              private string CreatedUsername;
              private string CreatedPassword;
              private const string FilePath = @"C:\Users\vanys\OneDrive\Desktop\Computer_science\VIRTUALSTUDIO\Hack Proof\Hack Proof\TextFile1.txt"; //pathway to the text document
              public LoginPage()
              {
                  InitializeComponent();
                  Pass_input.UseSystemPasswordChar = true;
                  Passverif_input.UseSystemPasswordChar = true; // Making the TextBox where the user enters the password be secure.
                  Pass_input.PasswordChar = '•';
                  Passverif_input.PasswordChar = '•';
                  Login_btn.Hide();
              }
      
              private void Login_btn_Click(object sender, EventArgs e)
              {
      
                  if (Pass_input.Text == CreatedPassword && User_input.Text == CreatedUsername) // Checks if the username and password is valid
                  {
                      if (Passverif_input.Text == Pass_input.Text)//check if the user typed the same password twice
                      {
                          Process.Start("notepad.exe", FilePath);// open the text document
                          Try_again();
                      }
                      else if (string.IsNullOrEmpty(Passverif_input.Text)) { MessageBox.Show("You Did Not Verify Your Password. Try Again."); Try_again(); } 
                      else { MessageBox.Show("Passwords Do Not Match."); Try_again(); }
                  }
                  else { MessageBox.Show("Invalid Login, Access Denied."); Try_again(); }
              }
      
      
      
      
              private void New_login_Click(object sender, EventArgs e)
              {
                  CreatedUsername = User_input.Text;// assign whatevert the user has entered to be the username and the password 
                  CreatedPassword = Pass_input.Text;
      
                  if (string.IsNullOrEmpty(CreatedUsername) || string.IsNullOrEmpty(CreatedPassword))// checks if the fields are empty or not
                  {
                      MessageBox.Show("Please do not leave the fields blank.");
                      return;
                  }
      
                  if (Validating_pass(CreatedPassword)) // Validates if the password meets all the needed standards
                  {
                      if (Passverif_input.Text == Pass_input.Text)//check if the user typed the same password twice
                      {
                          MessageBox.Show("Username and password set successfully.");
                          Try_again();
                          New_login.Hide();
                          info_lab.Hide();
                          Login_btn.Show();
                      }
                      else if (string.IsNullOrEmpty(Passverif_input.Text)) { MessageBox.Show("You Did Not Verify Your Password. Try Again."); Try_again(); }
                      else { MessageBox.Show("Passwords Do Not Match."); Try_again(); }
                      
                  }
                  else
                  {
                      MessageBox.Show("Password must be at least 8 characters long and include a lowercase letter, uppercase letter, and a special character.");
                      Try_again();
                      
                  }
       
      
              }
      
              private bool Validating_pass(string password)
              {
                  if (password.Length < 8)// Checks if the password has at least 8 characters
                  {
                      return false;
                  }
      
                  bool checklow = Regex.IsMatch(password, "[a-z]"); // Checks for lowercase 
                  bool checkup = Regex.IsMatch(password, "[A-Z]");// Checks for uppercase 
                  bool checkspec = Regex.IsMatch(password, "[!@#$%^&*(),.?\":{}|<>]"); // Checks for special character
      
                  return checklow && checkup && checkspec; // Returns True only if the conditions are met
              }
              
              private void Try_again() // Just Clears the TextBoxes
              {
                  User_input.Clear();
                  Pass_input.Clear();
                  Passverif_input.Clear();
              }
          }
      }
