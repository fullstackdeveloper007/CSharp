## Async Read data from DB
<pre><code>
using System;
using System.Data.SqlClient;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        await ReadDataAsync();
    }

    static async Task ReadDataAsync()
    {
        string connectionString = "Data Source=your_server;Initial Catalog=your_database;Integrated Security=True";
        string query = "SELECT * FROM YourTable";

        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            await connection.OpenAsync();

            using (SqlCommand command = new SqlCommand(query, connection))
            {
                using (SqlDataReader reader = await command.ExecuteReaderAsync())
                {
                    while (await reader.ReadAsync())
                    {
                        // Process the data
                        string data = reader["ColumnName"].ToString();
                        Console.WriteLine(data);
                    }
                }
            }
        }
    }
}

</code></pre>

## 2. Async Read data from DB --Returning DataTable
<pre><code>
using System;
using System.Data;
using System.Data.SqlClient;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        DataTable dataTable = await ReadDataAsync();
    }

    static async Task<DataTable> ReadDataAsync()
    {
        string connectionString = "Data Source=your_server;Initial Catalog=your_database;Integrated Security=True";
        string query = "SELECT * FROM YourTable";

        DataTable dataTable = new DataTable();

        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            await connection.OpenAsync();

            using (SqlCommand command = new SqlCommand(query, connection))
            {
                using (SqlDataReader reader = await command.ExecuteReaderAsync())
                {
                    dataTable.Load(reader);
                }
            }
        }

        return dataTable;
    }
}
</code></pre>
