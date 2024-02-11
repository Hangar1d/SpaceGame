import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

import javax.imageio.ImageIO;
import javax.swing.JFrame;
import javax.swing.JPanel;

class GraphicObject {
    BufferedImage img = null;
    int x = 0, y = 0;

    public GraphicObject(String name) {
        try {
            img = ImageIO.read(new File(name));
        } catch (IOException e) {
            System.out.println(e.getMessage());
            System.exit(0);
        }
    }

    public void update() {
    }

    public void draw(Graphics g) {
        g.drawImage(img, x, y, 100, 100, null);
    }

    public void keyPressed(KeyEvent event) {
    }
}

class Missile extends GraphicObject {
    boolean launched = false;

    public Missile(String name) {
        super(name);
        y = -200;
    }

    public void update() {
        if (launched)
            y -= 2;
        if (y < -100)
            launched = false;
    }

    public void keyPressed(KeyEvent event, int x, int y) {
        if (event.getKeyCode() == KeyEvent.VK_SPACE) {
            launched = true;
            this.x = x;
            this.y = y;
        }
    }
}

class Enemy extends GraphicObject {
    int dx = -5;
    int life = 5;

    public Enemy(String name) {
        super(name);
        x = 500;
        y = 30;
    }

    public void update() {
        x += dx;
        if (x < 0)
            dx = +3;
        if (x > 400)
            dx = -3;
    }
}

class SpaceShip extends GraphicObject {
    public SpaceShip(String name) {
        super(name);
        x = 150;
        y = 380;
    }

    public void keyPressed(KeyEvent event) {
        if (event.getKeyCode() == KeyEvent.VK_LEFT) {
            x -= 10;
        }
        if (event.getKeyCode() == KeyEvent.VK_RIGHT) {
            x += 10;
        }
        if (event.getKeyCode() == KeyEvent.VK_UP) {
            y -= 10;
        }
        if (event.getKeyCode() == KeyEvent.VK_DOWN) {
            y += 10;
        }
    }
}

class MyPanel extends JPanel implements KeyListener {
    Enemy enemy;
    SpaceShip spaceship;
    List<Missile> missiles;

    public MyPanel() {
        super();
        this.addKeyListener(this);
        this.requestFocus();
        setFocusable(true);

        enemy = new Enemy("enemy.png");
        spaceship = new SpaceShip("spaceship.png");
        missiles = new ArrayList<>();

        class MyThread extends Thread {
            public void run() {
                while (true) {
                    enemy.update();
                    spaceship.update();
                    updateMissiles();
                    repaint();
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                    }
                }
            }
        }

        Thread t = new MyThread();
        t.start();
    }

    private void updateMissiles() {
        for (Iterator<Missile> iterator = missiles.iterator(); iterator.hasNext();) {
            Missile missile = iterator.next();
            missile.update();

            if (missile.x < enemy.x + 70 && missile.x + 20 > enemy.x && missile.y < enemy.y + 70
                    && missile.y + 70 > enemy.y) {
                enemy.life--;
                iterator.remove();
            }

            if (!missile.launched) {
                iterator.remove();
            }
        }
    }

    public void paintComponent(Graphics g) {
        super.paintComponent(g);
        if (enemy.life > 0) {
            enemy.draw(g);
            g.drawImage(loadImage("heart.png"), getWidth() - 70, 5, 20, 20, null);
            g.setFont(new Font("Arial", Font.BOLD, 13));
            g.drawString(": " + enemy.life, getWidth() - 40, 20);
        }
        spaceship.draw(g);
        for (Missile missile : missiles) {
            missile.draw(g);
        }

        if (enemy.life <= 0) {
        	g.setColor(Color.RED);
            g.setFont(new Font("Arial", Font.BOLD, 40));
            g.drawString("Game Over!!!", 130, 200);
        }
    }

    private BufferedImage loadImage(String filename) {
        try {
            return ImageIO.read(new File(filename));
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

    public void keyPressed(KeyEvent event) {
        spaceship.keyPressed(event);
        if (event.getKeyCode() == KeyEvent.VK_SPACE) {
            Missile missile = new Missile("missile.png");
            missile.keyPressed(event, spaceship.x, spaceship.y);
            missiles.add(missile);
        }
    }

    public void keyReleased(KeyEvent arg0) {
    }

    public void keyTyped(KeyEvent arg0) {
    }
}

public class GallogGame extends JFrame {
    public GallogGame() {
        setTitle("My Game");
        add(new MyPanel());
        setSize(500, 500);
        setVisible(true);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new GallogGame();
    }
}
