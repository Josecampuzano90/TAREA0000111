using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

class Program
{
    static void Main()
    {
        // Generar ciudadanos
        HashSet<string> ciudadanos = new HashSet<string>();
        for (int i = 1; i <= 500; i++)
        {
            ciudadanos.Add($"Ciudadano {i}");
        }
        
        // Generar ciudadanos vacunados con Pfizer
        HashSet<string> vacunadosPfizer = GenerarVacunados(ciudadanos, 75);
        
        // Generar ciudadanos vacunados con AstraZeneca
        HashSet<string> vacunadosAstrazeneca = GenerarVacunados(ciudadanos, 75);
        
        // Ciudadanos que han recibido ambas vacunas
        HashSet<string> vacunadosAmbas = new HashSet<string>(vacunadosPfizer.Intersect(vacunadosAstrazeneca));
        
        // Ciudadanos que no se han vacunado
        HashSet<string> noVacunados = new HashSet<string>(ciudadanos.Except(vacunadosPfizer.Union(vacunadosAstrazeneca)));
        
        // Ciudadanos que han recibido las dos vacunas
        HashSet<string> vacunadosDosDosis = new HashSet<string>(vacunadosAmbas);
        
        // Ciudadanos que solo han recibido Pfizer
        HashSet<string> soloPfizer = new HashSet<string>(vacunadosPfizer.Except(vacunadosAstrazeneca));
        
        // Ciudadanos que solo han recibido AstraZeneca
        HashSet<string> soloAstrazeneca = new HashSet<string>(vacunadosAstrazeneca.Except(vacunadosPfizer));
        
        // Generar reporte
        string reporte = "Reporte de Vacunación COVID-19\n" +
                         "----------------------------------\n" +
                         $"Total ciudadanos: {ciudadanos.Count}\n" +
                         $"No vacunados: {noVacunados.Count}\n" +
                         $"Vacunados con ambas dosis: {vacunadosDosDosis.Count}\n" +
                         $"Vacunados solo con Pfizer: {soloPfizer.Count}\n" +
                         $"Vacunados solo con AstraZeneca: {soloAstrazeneca.Count}\n";
        
        File.WriteAllText("ReporteVacunacion.txt", reporte);
        Console.WriteLine("Reporte generado: ReporteVacunacion.txt");
    }
    
    static HashSet<string> GenerarVacunados(HashSet<string> ciudadanos, int cantidad)
    {
        Random rand = new Random();
        return new HashSet<string>(ciudadanos.OrderBy(x => rand.Next()).Take(cantidad));
    }
}
