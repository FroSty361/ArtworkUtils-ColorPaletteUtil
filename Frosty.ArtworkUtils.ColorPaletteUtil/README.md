Frosty.ArtworkUtils.ColorPaletteUtil

Utility Class For Colors. Part Of The ArtworksUtils Product

using System;
using System.Drawing;

public static class ColorPalette
{
    public static List<string> GetColorsHex(string imageLocation, int width = 0, int height = 0)
    {
      CheckIfImageExists(imageLocation);

      var original = new Bitmap(imageLocation);

      if (width == 0 && height == 0)
      {
        width = original.Width / 10;

        height = original.Height / 10;
      }

      Color color;

      List<string> hexColors = new List<string>();

      var image = new Bitmap(original, new Size(width, height));

      for (int y = 0; y < image.Height; y++)
      {
        for (int x = 0; x < image.Width; x++)
        {
          color = image.GetPixel(x, y);

          hexColors.Add(ColorTranslator.ToHtml(color));
        }
      }

      return hexColors;
    }

    public static List<Color> GetColors(string imageLocation, int width = 0, int height = 0)
    {
      CheckIfImageExists(imageLocation);

      var original = new Bitmap(imageLocation);

      if (width == 0 && height == 0)
      {
        width = original.Width / 10;

        height = original.Height / 10;
      }

      List<Color> colors = new List<Color>();

      var image = new Bitmap(original, new Size(width, height));

      for (int y = 0; y < image.Height; y++)
      {
        for (int x = 0; x < image.Width; x++)
        {
          colors.Add(image.GetPixel(x, y));
        }
      }

      return colors;
    }

    public static string GetAverageColorHex(string imageLocation, int width = 0, int height = 0)
    {
        var colors = GetColors(imageLocation, width, height);

        long totalR = 0, totalG = 0, totalB = 0;
        int count = colors.Count;

        if (count == 0)
        {
          throw new InvalidOperationException("No colors found in the image.");
        }

        foreach (var color in colors)
        {
            totalR += color.R;
            totalG += color.G;
            totalB += color.B;
        }

        var avgColor = Color.FromArgb(
            (int)(totalR / count),
            (int)(totalG / count),
            (int)(totalB / count)
        );

        return ColorTranslator.ToHtml(avgColor);
    }

    public static Color GetAverageColor(string imageLocation, int width = 0, int height = 0)
    {
        var colors = GetColors(imageLocation, width, height);

        long totalR = 0, totalG = 0, totalB = 0;
        int count = colors.Count;

        if (count == 0)
        {
          throw new InvalidOperationException("No colors found in the image.");
        }

        foreach (var color in colors)
        {
            totalR += color.R;
            totalG += color.G;
            totalB += color.B;
        }

        return Color.FromArgb(
            (int)(totalR / count),
            (int)(totalG / count),
            (int)(totalB / count)
        );
    }

    private static void CheckIfImageExists(string imageLocation)
    {
      if (!File.Exists(imageLocation))
      {
          throw new FileNotFoundException("Image file not found!", imageLocation);
      }
    }

    public static string GetColorComplementaryHex(string hex)
    {
      Color complementaryColor = GetColorComplementary(hex);

      return ColorTranslator.ToHtml(complementaryColor);
    }

    public static Color GetColorComplementary(string hex)
    {
      Color color = ColorTranslator.FromHtml(hex);
      
      int r = 255 - Convert.ToInt16(color.R);
      int g = 255 - Convert.ToInt16(color.G);
      int b = 255 - Convert.ToInt16(color.B);

      Color complementaryColor = Color.FromArgb(r, g, b);

      return complementaryColor;
    }
}