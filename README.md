# actividad-ppb
import java.awt.*;
import java.awt.geom.*;
import java.io.*;
import java.util.*;

public class MapDrawing {

    private static final int PADDING = 10;
    private static final int FONT_SIZE = 12;
    private static final Color BACKGROUND_COLOR = Color.white;
    private static final Color FOREGROUND_COLOR = Color.black;
    private static final Color EDGE_COLOR = Color.blue;

    private static final String FONT_NAME = "Arial";

    private final Map<String, Point2D> locations;

    public MapDrawing(Map<String, Point2D> locations) {
        this.locations = locations;
    }

    public void draw(Graphics2D g) {
        g.setColor(BACKGROUND_COLOR);
        g.fillRect(0, 0, getWidth(), getHeight());

        g.setColor(FOREGROUND_COLOR);
        g.setFont(new Font(FONT_NAME, Font.PLAIN, FONT_SIZE));

        for (Map.Entry<String, Point2D> entry : locations.entrySet()) {
            String name = entry.getKey();
            Point2D location = entry.getValue();

            g.drawString(name, location.getX() + PADDING, location.getY() + PADDING);
            g.drawOval(location.getX() - PADDING, location.getY() - PADDING, 2 * PADDING, 2 * PADDING);
        }

        g.setColor(EDGE_COLOR);
        g.drawRect(0, 0, getWidth() - 1, getHeight() - 1);
    }

    public int getWidth() {
        int width = 0;
        for (Map.Entry<String, Point2D> entry : locations.entrySet()) {
            Point2D location = entry.getValue();
            width = Math.max(width, (int)location.getX() + PADDING);
        }
        return width + PADDING;
    }

    public int getHeight() {
        int height = 0;
        for (Map.Entry<String, Point2D> entry : locations.entrySet()) {
            Point2D location = entry.getValue();
            height = Math.max(height, (int)location.getY() + PADDING);
        }
        return height + PADDING;
    }

    public void save(String filename) throws IOException {
        BufferedImage image = new BufferedImage(getWidth(), getHeight(), BufferedImage.TYPE_INT_RGB);
        Graphics2D g = image.createGraphics();
        draw(g);
        g.dispose();

        ImageIO.write(image, "png", new File(filename));
    }

    public static void main(String[] args) throws IOException {
        Map<String, Point2D> locations = new HashMap<>();
        locations.put("Juntas de acción comunal", new Point2D.Double(100, 100));
        locations.put("Alcaldía local - Casa de la juventud", new Point2D.Double(200, 200));
        locations.put("Casa de participación o personerías locales", new Point2D.Double(300, 300));
        locations.put("Registraduría Nacional", new Point2D.Double(400, 400));
        locations.put("SUPERCADE", new Point2D.Double(500, 500));
        locations.put("Casas culturales, casas de la igualdad, Centros de Desarrollo Comunitarios, etc.", new Point2D.Double(600, 600));
        locations.put("Espacios públicos de esparcimiento y aprovechamiento del tiempo libre", new Point2D.Double(700, 700));
        locations.put("La manzana del cuidado de su localidad", new Point2D.Double(800, 800));

        MapDrawing drawing = new MapDrawing(locations);
        drawing.save("map.png");
    }
}
