C# + SQL Server: Fundamentos e Integração Backend
Contexto e Objetivos

Este projeto tem como objetivo utilizar a Inteligência Artificial como ferramenta de aprendizado ativo por meio do NotebookLM, organizando e consolidando conhecimentos sobre desenvolvimento backend com C# (.NET) e SQL Server.

O tema escolhido foi:

"Fundamentos de C# com SQL Server e integração de banco de dados em aplicações backend"

Objetivos do estudo:
Entender como funciona a conexão entre C# e SQL Server
Aprender operações CRUD (Create, Read, Update, Delete)
Estudar uso de ADO.NET e Entity Framework Core
Compreender comandos SQL básicos aplicados em sistemas reais
Estruturar boas práticas de backend em .NET
Criar um material de revisão reutilizável para estudos futuros

Curadoria de Fontes

As fontes estão no arquivo fontes.md.

Engenharia de Prompts e Iterações

Os prompts utilizados estão documentados no arquivo prompts.md.

Miniguia de Estudo
Conexão C# com SQL Server

A conexão com o banco é feita através de uma connection string:

string connectionString =
"Server=DESKTOP-XXXX;Database=MinhaBase;Trusted_Connection=True;";

Exemplo ADO.NET (CRUD básico)
using System;
using System.Data.SqlClient;

string connectionString =
"Server=DESKTOP-XXXX;Database=MinhaBase;Trusted_Connection=True;";

SqlConnection conn = new SqlConnection(connectionString);

try
{
    conn.Open();

    SqlCommand cmd = new SqlCommand("SELECT * FROM Produtos", conn);

    SqlDataReader reader = cmd.ExecuteReader();

    while (reader.Read())
    {
        Console.WriteLine(reader["Nome"]);
    }
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
finally
{
    conn.Close();
}

Entity Framework Core

Model

public class Produto
{
    public int Id { get; set; }
    public string Nome { get; set; }
    public decimal Preco { get; set; }
}
DbContext

using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public DbSet<Produto> Produtos { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
        options.UseSqlServer(
            "Server=DESKTOP-XXXX;Database=MinhaBase;Trusted_Connection=True;"
        );
    }
}

Controller (API)

using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Linq;

[ApiController]
[Route("api/produtos")]
public class ProdutosController : ControllerBase
{
    private readonly AppDbContext _context;

    public ProdutosController(AppDbContext context)
    {
        _context = context;
    }

    [HttpGet]
    public IEnumerable<Produto> Get()
    {
        return _context.Produtos.ToList();
    }

    [HttpPost]
    public IActionResult Post(Produto produto)
    {
        _context.Produtos.Add(produto);
        _context.SaveChanges();
        return Ok(produto);
    }
}

Comparação rápida

ADO.NET → acesso direto ao banco, mais manual
Entity Framework Core → ORM moderno, mais produtivo

Glossário

SQL Server → banco de dados relacional da Microsoft
C# → linguagem backend da Microsoft
ADO.NET → acesso direto ao banco
Entity Framework Core → ORM moderno
DbContext → classe de conexão com o banco
CRUD → Create, Read, Update, Delete
Connection String → string de conexão com banco

Conclusão

Este projeto demonstra a integração entre C# e SQL Server utilizando abordagens modernas e tradicionais de acesso a dados.

O uso do NotebookLM ajudou a estruturar o aprendizado e transformar conceitos técnicos em um material organizado e reutilizável.
