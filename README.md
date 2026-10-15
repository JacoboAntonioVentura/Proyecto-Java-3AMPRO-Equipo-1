package bienvenidos;

import javax.swing.*;
import java.awt.*;
import java.net.URL;
import java.util.ArrayList;
import java.io.FileWriter;
import java.io.PrintWriter;
import java.io.IOException;

public class MembresiaGymImage {

    public static void main(String[] args) {

        URL url = MembresiaGymImage.class.getResource("Gym1.png");

        ImageIcon icono = null;
        if (url != null) {
            ImageIcon original = new ImageIcon(url);
            Image img = original.getImage().getScaledInstance(
                    80, 80, Image.SCALE_SMOOTH);
            icono = new ImageIcon(img);
        }

        UIManager.put("OptionPane.background", new Color(45, 45, 45));
        UIManager.put("Panel.background", new Color(70, 70, 70));
        UIManager.put("OptionPane.messageForeground", Color.WHITE);

        double mensualidad = 450;

        ArrayList<String> historialTransacciones = new ArrayList<>();

        ArrayList<String> historialRutinas = new ArrayList<>();

        String nombre = entrada("Ingresa tu nombre:", "Inicio", icono);

        if (nombre == null || nombre.trim().isEmpty()) {
            nombre = "Cliente";
        }

        mensaje("Bienvenido al gimnasio el Chorrazo, " + nombre, icono);

        while (true) {

            String op = entrada("""
                    ---- MENU MEMBRESIA GIMNASIO ----
                    1. Pago mensual
                    2. Pago anual con descuento
                    3. Recargo por pago tardío
                    4. Entrenador personal
                    5. Rutina de brazo
                    6. Rutina de hombro
                    7. Rutina de pecho
                    8. Rutina de pierna
                    9. Ver historial de pagos (Arreglo)
                    10. Salir y guardar (Archivo)
                    11. Ver historial de rutinas
                    """, "Menú", icono);

            if (op == null) {
                break;
            }

            int opcion;

            try {
                opcion = Integer.parseInt(op);
            } catch (NumberFormatException e) {
                mensaje("Por favor ingresa un número válido.", icono);
                continue;
            }

            if (opcion == 1) {

                mensaje("Mensualidad: $" + mensualidad, icono);

                double total = mensualidad;
                String metodo = metodoPago(total, icono);

                if (metodo != null) {
                    historialTransacciones.add(
                            "Cliente: " + nombre
                            + " - Pago Mensual: $" + total
                            + " (" + metodo + ")");
                }

            } else if (opcion == 2) {

                double total = mensualidad * 12 * 0.8;

                mensaje("Total anual con descuento: $" + total, icono);

                String metodo = metodoPago(total, icono);

                if (metodo != null) {
                    historialTransacciones.add(
                            "Cliente: " + nombre
                            + " - Pago Anual: $" + total
                            + " (" + metodo + ")");
                }

            } else if (opcion == 3) {

                String d = entrada("Días de retraso:", "Entrada", icono);

                if (d != null) {
                    try {
                        int dias = Integer.parseInt(d);

                        if (dias > 0) {
                            double total = mensualidad
                                    + (mensualidad * 0.10);

                            mensaje("Total con recargo: $" + total, icono);

                            String metodo = metodoPago(total, icono);

                            if (metodo != null) {
                                historialTransacciones.add(
                                        "Cliente: " + nombre
                                        + " - Pago con Recargo (" + dias
                                        + " días): $" + total
                                        + " (" + metodo + ")");
                            }
                        } else {
                            mensaje("No hay recargo", icono);
                        }

                    } catch (NumberFormatException e) {
                        mensaje("Ingresa un número válido de días.", icono);
                    }
                }

            } else if (opcion == 4) {

                String h = entrada("Horas de entrenador:", "Entrada", icono);

                if (h != null) {
                    try {
                        int horas = Integer.parseInt(h);

                        if (horas <= 0) {
                            mensaje("Las horas deben ser mayores que cero.", icono);
                            continue;
                        }

                        double total = horas * 200;

                        mensaje("Total entrenador: $" + total, icono);

                        String metodo = metodoPago(total, icono);

                        if (metodo != null) {
                            historialTransacciones.add(
                                    "Cliente: " + nombre
                                    + " - Entrenador (" + horas
                                    + " hrs): $" + total
                                    + " (" + metodo + ")");
                        }

                    } catch (NumberFormatException e) {
                        mensaje("Ingresa un número válido de horas.", icono);
                    }
                }

            } else if (opcion == 5) {

                URL brazoURL = MembresiaGymImage.class.getResource("Brazo.jpg");
                ImageIcon brazoIcon = getScaledIcon(brazoURL);

                historialRutinas.add("Rutina de brazo");

                mensaje("""
                        RUTINA DE BRAZO
                        1. Curl de bíceps - 4x12
                        2. Martillo - 4x10
                        3. Fondos tríceps - 4x15
                        4. Extensión tríceps - 4x12
                        """, brazoIcon);

            } else if (opcion == 6) {

                URL hombroURL = MembresiaGymImage.class.getResource("Hombro.jpg");
                ImageIcon hombroIcon = getScaledIcon(hombroURL);

                // NUEVO: Registrar la rutina consultada
                historialRutinas.add("Rutina de hombro");

                mensaje("""
                        RUTINA DE HOMBRO
                        1. Press militar - 4x10
                        2. Elevaciones laterales - 4x12
                        3. Elevaciones frontales - 4x12
                        4. Extensión con polea - 4x15
                        """, hombroIcon);

            } else if (opcion == 7) {

                URL pechoURL = MembresiaGymImage.class.getResource("Pecho.png");
                ImageIcon pechoIcon = getScaledIcon(pechoURL);

                // NUEVO: Registrar la rutina consultada
                historialRutinas.add("Rutina de pecho");

                mensaje("""
                        RUTINA DE PECHO
                        1. Press banca - 4x10
                        2. Aperturas - 4x12
                        3. Flexiones - 4x20
                        4. Press inclinado - 4x10
                        """, pechoIcon);

            } else if (opcion == 8) {

                URL piernaURL = MembresiaGymImage.class.getResource("Pierna.png");
                ImageIcon piernaIcon = getScaledIcon(piernaURL);

                historialRutinas.add("Rutina de pierna");

                mensaje("""
                        RUTINA DE PIERNA
                        1. Sentadilla - 4x12
                        2. Prensa - 4x10
                        3. Desplantes - 4x12
                        4. Elevación pantorrilla - 4x20
                        """, piernaIcon);

            } else if (opcion == 9) {

                if (historialTransacciones.isEmpty()) {
                    mensaje("No hay transacciones registradas aún.", icono);
                } else {
                    StringBuilder sb = new StringBuilder(
                            "---- HISTORIAL DE TRANSACCIONES ----\n\n");

                    for (String transaccion : historialTransacciones) {
                        sb.append("- ").append(transaccion).append("\n");
                    }

                    mensaje(sb.toString(), icono);
                }

            } else if (opcion == 10) {

                guardarHistorialEnArchivo(historialTransacciones);

                mensaje(
                        "Saliste del programa. Historial guardado en "
                        + "'historial_gimnasio.txt'.",
                        icono);

                break;

            } else if (opcion == 11) {

                if (historialRutinas.isEmpty()) {
                    mensaje("No has consultado ninguna rutina.", icono);
                } else {
                    StringBuilder sb = new StringBuilder(
                            "---- HISTORIAL DE RUTINAS ----\n\n");

                    for (String rutina : historialRutinas) {
                        sb.append("- ").append(rutina).append("\n");
                    }

                    mensaje(sb.toString(), icono);
                }

            } else {
                mensaje("Opción inválida", icono);
            }
        }
    }

    public static void mensaje(String texto, ImageIcon icono) {
        JOptionPane.showMessageDialog(
                null,
                texto,
                "Gimnasio",
                JOptionPane.INFORMATION_MESSAGE,
                icono
        );
    }

    public static String entrada(
            String texto, String titulo, ImageIcon icono) {

        return (String) JOptionPane.showInputDialog(
                null,
                texto,
                titulo,
                JOptionPane.QUESTION_MESSAGE,
                icono,
                null,
                null
        );
    }

    public static String metodoPago(double total, ImageIcon icono) {

        String op = entrada("""
                ¿Cómo deseas pagar?
                1. Efectivo
                2. Tarjeta
                """, "Pago", icono);

        if (op == null) {
            return null;
        }

        try {
            int opcion = Integer.parseInt(op);

            if (opcion == 1) {
                mensaje("Pagaste en efectivo\nTotal: $" + total, icono);
                return "Efectivo";

            } else if (opcion == 2) {
                mensaje("Pagaste con tarjeta\nTotal: $" + total, icono);
                return "Tarjeta";

            } else {
                mensaje("Opción no válida", icono);
                return null;
            }

        } catch (NumberFormatException e) {
            mensaje("Ingresa una opción de pago válida.", icono);
            return null;
        }
    }

    public static ImageIcon getScaledIcon(URL url) {

        if (url == null) {
            return null;
        }

        Image imagen = new ImageIcon(url).getImage()
                .getScaledInstance(120, 120, Image.SCALE_SMOOTH);

        return new ImageIcon(imagen);
    }

    public static void guardarHistorialEnArchivo(
            ArrayList<String> historial) {

        try (PrintWriter writer = new PrintWriter(
                new FileWriter("historial_gimnasio.txt", true))) {

            writer.println("=== NUEVA SESIÓN DE REGISTROS ===");

            for (String item : historial) {
                writer.println(item);
            }

            writer.println("----------------------------------------\n");

        } catch (IOException e) {
            System.err.println(
                    "Error al guardar el archivo: " + e.getMessage());
        }
    }
}
