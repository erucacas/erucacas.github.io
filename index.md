

	import java.awt.Color;
	import java.awt.Graphics;

	public class ObeloFigura {
	
	protected int xIni,
				  yIni, 
				  ancho, 
				  alto;
	private Color colorBarra;
	private double pBarraAlto, 
				   pBolaAlto;
	
	public ObeloFigura(int xIni, int yIni, int ancho, int alto, Color colorBarra) {
		this.xIni = xIni;
		this.yIni = yIni;
		this.ancho = ancho;
		this.alto = alto;
		this.colorBarra=colorBarra;
		this.pBarraAlto = 0.20;
		this.pBolaAlto = 0.60;
	}

	public ObeloFigura(int xIni, int yIni, int ancho, int alto) {
		this(xIni, yIni, ancho, alto, Color.GREEN);
	}
	
	//Getters
	public int getXInt() {
		return this.xIni;
	}
	public int getYInt() {
		return this.yIni;
	}
	public int getAncho() {
		return this.ancho;
	}
	public int getAlto() {
		return this.alto;
	}
	public Color getColorBarra() {
		return this.colorBarra;
	}
	public double getpBarraAlto() {
		return this.pBarraAlto;
	}
	
	public double getpBolaAlto() {
		return this.pBolaAlto;
	}
	
	public void setXYInt(int xIni, int yIni) {
		this.xIni = xIni;
		this.yIni = yIni;
	}
	
	public void setXYFin(int xFin, int yFin) {
		this.ancho=xFin-this.xIni;
		this.alto=yFin-this.yIni;
	}
	
	public void setcolorBarra(Color colorBarra) {
		this.colorBarra = colorBarra;
	}
	
	public void setpBolaAlto(double pBolaAlto) {
		this.pBolaAlto = pBolaAlto;
	}
	
	public void pintaObelo(Graphics g) {
		double pLibre = (1.0-this.pBarraAlto)/2.0;
		double anchoLibre = pLibre*this.alto;
		double diametroBola = anchoLibre * this.pBolaAlto;
		int yBarra = (int)(this.yIni+this.alto*pLibre);
		int xBola = (int)(this.xIni+(this.ancho - diametroBola)/2.0);
		g.setColor(this.colorBarra);
		g.fillRect( this.xIni, yBarra, this.ancho, (int)(this.alto*this.pBarraAlto));
		g.setColor(Color.GREEN);
		g.fillOval(xBola, this.yIni,(int)(pLibre*this.alto*this.pBolaAlto), (int)(pLibre*this.alto*this.pBolaAlto));
		g.fillOval (xBola, (int)(this.yIni+this.alto-diametroBola),(int)(pLibre*this.alto*this.pBolaAlto),(int)(pLibre*this.alto*this.pBolaAlto));
		
	}
	

	}

<hr>

	import java.awt.Color;
	import java.awt.Dimension;
	import java.awt.Graphics;
	import java.awt.event.MouseEvent;
	import java.awt.event.MouseListener;
	import java.awt.event.MouseMotionListener;
	import javax.swing.JColorChooser;
	import javax.swing.JPanel;

	public class PanelObelo extends JPanel implements MouseListener, MouseMotionListener{

	private ObeloFigura obelo;

	public PanelObelo() {
		super();
		this.setPreferredSize(new Dimension(800,600));
		this.obelo = new ObeloFigura(0,0,0,0);
		this.addMouseListener(this);
		this.addMouseMotionListener(this);
	}
	
	public ObeloFigura getObelo() {
		return this.obelo;
	}
	
	public void pintarObelo(Graphics g) {
		this.obelo.pintaObelo(g);
	}

	public void paintComponent(Graphics g) {
		super.paintComponent(g);
		pintarObelo(g);
		ObeloFiguraMarco om = new ObeloFiguraMarco(this.obelo.getXInt(),this.obelo.getYInt(),this.obelo.getAncho(),this.obelo.getAlto());
		om.pintaMarco(g);
	}
	
	@Override
	public void mouseDragged(MouseEvent e) {
		this.obelo.setXYFin(e.getX(),e.getY());
		System.out.println(e.getX()+ "," +e.getY());
		this.repaint();
	}

	@Override
	public void mouseMoved(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void mouseClicked(MouseEvent e) {
		// TODO Auto-generated method stub
		Color tmpColor = JColorChooser.showDialog(this, "Selecciona un color", Color.RED);
		this.obelo.setcolorBarra(tmpColor);
	}

	@Override
	public void mousePressed(MouseEvent e) {
		this.obelo.setXYInt(e.getX(), e.getY());
		//System.out.println(e.getX()+ "," + e.getY());
		
	}

	@Override
	public void mouseReleased(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void mouseEntered(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void mouseExited(MouseEvent e) {
		// TODO Auto-generated method stub
		
	}
		
	}


<hr>

	import java.awt.BorderLayout;

	import javax.swing.JFrame;

	public class VentanaObelo extends JFrame{

	public VentanaObelo(PanelObelo po) {
		super("Obelo");
		this.add(po);
		this.add(new PanelControlesObelo(po),BorderLayout.SOUTH);
		this.setDefaultCloseOperation(EXIT_ON_CLOSE);
		this.pack();
		this.setVisible(true);
		
	}
	
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		VentanaObelo poo = new VentanaObelo(new PanelObelo());
	}

}


<hr>

	import java.awt.Color;
	import java.awt.event.ActionEvent;
	import java.awt.event.ActionListener;
	import java.io.BufferedReader;
	import java.io.FileNotFoundException;
	import java.io.FileReader;
	import java.io.FileWriter;
	import java.io.IOException;
	import java.io.PrintWriter;

	import javax.swing.JButton;
	import javax.swing.JPanel;
	import javax.swing.JSlider;
	import javax.swing.event.ChangeEvent;
	import javax.swing.event.ChangeListener;

	public class PanelControlesObelo extends JPanel implements ActionListener,ChangeListener{
	private JSlider slider;
	private JButton btnguardar,
			btnabrir;
	private PanelObelo po;
	
	public PanelControlesObelo(PanelObelo po) {
		super();
		
		this.po=po;
		
		slider=new JSlider(JSlider.HORIZONTAL,0,100,60);
		this.slider.setPaintLabels(true);
		this.slider.setPaintTicks(true);
		this.slider.setMajorTickSpacing(20);
		this.slider.setMinorTickSpacing(5);
		this.add(this.slider);
		this.slider.addChangeListener(this);
		
		btnguardar=new JButton("Guardar");
		this.add(btnguardar);
		this.btnguardar.addActionListener(this);
		
		btnabrir=new JButton("Abrir");
		this.add(btnabrir);
		this.btnabrir.addActionListener(this);
	}
	
	@Override
	public void stateChanged(ChangeEvent arg0) {
		this.po.getObelo().setpBolaAlto(this.slider.getValue()/100.0);
		po.repaint();
		
	}

	@Override
	public void actionPerformed(ActionEvent evt) {
		
					
			if(evt.getSource()==btnguardar) {
				try {
				PrintWriter pw=new PrintWriter(new FileWriter("Obelo.txt"));
				pw.println(this.po.getObelo().getXInt());
				pw.println(this.po.getObelo().getYInt());
				pw.println(this.po.getObelo().getAncho());
				pw.println(this.po.getObelo().getAlto());
				pw.println(this.po.getObelo().getpBolaAlto());
				pw.println(this.po.getObelo().getColorBarra().getRed());
				pw.println(this.po.getObelo().getColorBarra().getGreen());
				pw.println(this.po.getObelo().getColorBarra().getBlue());
				pw.close();
				} catch(FileNotFoundException ex) {
					System.out.println("Valiste vrg");
				} catch(IOException e) {
					System.out.println("Valiste VRG");
				} 
			}else {
				try {
				BufferedReader br=new BufferedReader(new FileReader("Obelo.txt"));
				this.po.getObelo().setXYInt(Integer.parseInt(br.readLine()), Integer.parseInt(br.readLine()));
				this.po.getObelo().setXYFin(Integer.parseInt(br.readLine()), Integer.parseInt(br.readLine()));
				this.po.getObelo().setpBolaAlto(Double.parseDouble(br.readLine()));
				this.po.getObelo().setcolorBarra(new Color(Integer.parseInt(br.readLine()), Integer.parseInt(br.readLine()), Integer.parseInt(br.readLine())));
				this.po.repaint();
				}catch(IOException e) {
					System.out.println("Valiste VRG");
				} 
			}
		
		} 
		
	}



<hr>

	import java.awt.Color;
	import java.awt.Graphics;

	public class ObeloFiguraMarco extends ObeloFigura{
	
	public ObeloFiguraMarco(int xIni,int yIni,int ancho,int alto) {
		super(xIni,yIni,ancho,alto);
	}
	
	public void pintaMarco(Graphics g) {
		g.setColor(Color.RED);
		g.drawRect(this.xIni,this.yIni,this.ancho,this.alto);
		
	}
	
	
	
	
}

<hr>

	import java.awt.Dimension;
	import java.awt.Graphics;

	import javax.swing.JPanel;

	public class Poligono extends JPanel{
	
	
	public Poligono() {
		super();
		this.setPreferredSize(new Dimension(800, 800));
		
	}
	
	public void dibujaPentagono(Graphics g) {
		int[] xs= {500,600,700,550,400},
			  ys= {700,700,600,500,600};
		g.fillPolygon(xs, ys, 5);
	}
	
	public void paintComponent(Graphics g) {
		this.dibujaPentagono(g);
	}
}

<hr>

	import javax.swing.JFrame;

	public class PoligonoVentana extends JFrame{

	public PoligonoVentana(Poligono p) {
		super();
		this.add(p);
		this.pack();
		this.setDefaultCloseOperation(EXIT_ON_CLOSE);
		this.setVisible(true);
	}
	
	public static void main(String[] args) {
		PoligonoVentana pv=new PoligonoVentana(new Poligono());
	}
}
