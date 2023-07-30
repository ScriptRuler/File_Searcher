# File_Searcher [ Commit # 1 ]
using System;
using System.Diagnostics;
using System.IO;

class Program
{
    static void Main()
    {
        Console.WriteLine("Search Keyword:");
        string wordToSearch = Console.ReadLine();
        Console.WriteLine("Please Enter Folder Path:");
        string folderPath = Console.ReadLine();
        string outputFileName = wordToSearch + ".txt";
        string outputFilePath = Path.Combine(folderPath, outputFileName);

        try
        {
            string[] files = Directory.GetFiles(folderPath, "*", SearchOption.AllDirectories);

            using (StreamWriter outputFile = new StreamWriter(outputFilePath))
            {
                foreach (string file in files)
                {
                    if (FileContainsWord(file, wordToSearch))
                    {
                        string fileName = Path.GetFileName(file);
                        outputFile.WriteLine(fileName);
                    }
                }
            }
            Console.WriteLine();
            Console.WriteLine($"Search completed. Results written to {outputFilePath}.");
            Console.ReadLine();
            string outputDirectory = Path.GetDirectoryName(outputFilePath);
            Process.Start("explorer.exe", outputDirectory);
        }
        catch (Exception ex)
        {
            Console.WriteLine("An error occurred: " + ex.Message);
        }
    }

    static bool FileContainsWord(string filePath, string wordToSearch)
    {
        try
        {
            using (StreamReader reader = new StreamReader(filePath))
            {
                string content = reader.ReadToEnd();
                return content.Contains(wordToSearch);
            }
        }
        catch
        {
            return false;
        }
    }
}
