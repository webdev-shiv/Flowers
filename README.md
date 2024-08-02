# Flowers
@@ -1,314 +1,298 @@
import java.awt.Color;
import java.awt.Graphics;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.util.Random;
import javax.imageio.ImageIO;

public class Flowergen {
	private static boolean[][] generateQuarter(int size){
		//Generates a random noise quarter for the flower
		boolean[][] ret = new boolean[size][size];
		for (int x=0; x<size; x++){
			for (int y=0; y<size; y++){
				ret[x][y] = new Random().nextBoolean();

	private static boolean[][] generateQuarter(int size) {
		// Generates a random noise quarter for the flower
		boolean[][] quarter = new boolean[size][size];
		for (int x = 0; x < size; x++) {
			for (int y = 0; y < size; y++) {
				quarter[x][y] = new Random().nextBoolean();
			}
		}
		return ret; 
		return quarter;
	}

	private static int adj(int i, int low, int high){
		//calls Math.min(high, Math.max(i, low))
		if (i<low) return low;
		if (i>high) return high;

	private static int applyBounds(int i, int lowBound, int highBound) {
		// Applies bounds to an int; equivalent to Math.min(high, Math.max(i, low))
		if (i < lowBound)
			return lowBound;
		if (i > highBound)
			return highBound;
		return i;
	}
	
	private static int adj(int i){
		//calls adj with most common values
		return adj(i, 0, 255);

	private static int applyBounds(int i) {
		// Applies bounds for the highest and lowest values a color is permitted to be.
		return applyBounds(i, 0, 255);
	}

	private static Color[][] colorizeFlower(boolean[][] flower){
		//Colorizes a flower. Only works on non-sector flowers 
		Color[][] ret = new Color[flower.length][flower[0].length];
		//find the key quarter
		Color[][] quarter = new Color[flower.length/2][flower[0].length/2];
		//select a base petal color - fully random except that it can't be too dark
	private static Color[][] colorizeFlower(boolean[][] flower) {
		// Colorizes a non-sector color.
		Color[][] result = new Color[flower.length][flower[0].length];
		Color[][] quarter = new Color[flower.length / 2][flower[0].length / 2];

		Random r = new Random();
		int pos = r.nextInt(2);
		int[] base = {r.nextInt(255),r.nextInt(255),r.nextInt(255)};
		//int[] accent = {r.nextInt(255),r.nextInt(255),r.nextInt(255)};
		while (base[0]+base[1]+base[2] < 50){
			base[0] = r.nextInt(255); base[1] = r.nextInt(255); base[2] = r.nextInt(255);	
		int[] base = { r.nextInt(255), r.nextInt(255), r.nextInt(255) };

		while (base[0] + base[1] + base[2] < 50) {
			base[0] = r.nextInt(255);
			base[1] = r.nextInt(255);
			base[2] = r.nextInt(255);
		}
		//create the "base quarter"
		int dist;
		for (int x=0; x<quarter.length; x++){
			for (int y=0; y<quarter[0].length; y++){
				if (pos==0) dist = x+y;
				else dist = -x-y;
				dist = dist*10; 
				if (flower[x][y]){
					quarter[x][y] = new Color(adj(base[0]+dist), adj(base[1]+dist), adj(base[2]+dist));
				}
				else quarter[x][y] = new Color(0,0,0);

		int distance;
		for (int x = 0; x < quarter.length; x++) {
			for (int y = 0; y < quarter[0].length; y++) {
				if (pos == 0)
					distance = x + y;
				else
					distance = -x - y;
				distance = distance * 10;
				if (flower[x][y]) {
					quarter[x][y] = new Color(applyBounds(base[0] + distance), applyBounds(base[1] + distance),
							applyBounds(base[2] + distance));
				} else
					quarter[x][y] = new Color(0, 0, 0);
			}
		}
		Color[][] quarter2 = rotate(quarter);
		Color[][] quarter3 = rotate(quarter2);
		Color[][] quarter4 = rotate(quarter3);
		for (int x=0; x<quarter.length; x++){
			for (int y=0; y<quarter[0].length; y++){
				ret[x][y] = quarter[x][y];
				ret[x+quarter.length][y] = quarter4[x][y];
				ret[x+quarter.length][y+quarter.length] = quarter3[x][y];
				ret[x][y+quarter.length] = quarter2[x][y]; 
		for (int x = 0; x < quarter.length; x++) {
			for (int y = 0; y < quarter[0].length; y++) {
				result[x][y] = quarter[x][y];
				result[x + quarter.length][y] = quarter4[x][y];
				result[x + quarter.length][y + quarter.length] = quarter3[x][y];
				result[x][y + quarter.length] = quarter2[x][y];
			}
		}
		dist=0;
		return ret; 
		return result;
	}
	

	private static Color[][] rotate(Color[][] array) {
	//Rotates a color array
		Color[][] ret = new Color[array[0].length][array.length];
	      for(int i = 0; i < array[0].length;i++) {
	         for(int j = 0; j < array.length; j++) {
	            ret[i][j] = array[array.length - j - 1][i];
	         }
	      }
	      return ret;
		Color[][] result = new Color[array[0].length][array.length];
		for (int i = 0; i < array[0].length; i++) {
			for (int j = 0; j < array.length; j++) {
				result[i][j] = array[array.length - j - 1][i];
			}
		}
		return result;
	}

	private static boolean[][] rotate(boolean[][] array) {
	      boolean[][] ret = new boolean[array[0].length][array.length];
	      for(int i = 0; i < array[0].length;i++) {
	         for(int j = 0; j < array.length; j++) {
	            ret[i][j] = array[array.length - j - 1][i];
	         }
	      }
	      return ret;
	   }

	private static boolean[][] generateFlower(int size){
		//Generates a bool flower. 
		boolean[][] result = new boolean[array[0].length][array.length];
		for (int i = 0; i < array[0].length; i++) {
			for (int j = 0; j < array.length; j++) {
				result[i][j] = array[array.length - j - 1][i];
			}
		}
		return result;
	}

	private static boolean[][] generateFlower(int size) {
		boolean[][] quarter1 = generateQuarter(size);
		boolean[][] quarter2 = rotate(quarter1);
		boolean[][] quarter3 = rotate(quarter2);
		boolean[][] quarter4 = rotate(quarter3);
		boolean[][] ret = new boolean[size*2][size*2];
		for (int x=0; x<size; x++){
			for (int y=0; y<size; y++){
		boolean[][] ret = new boolean[size * 2][size * 2];
		for (int x = 0; x < size; x++) {
			for (int y = 0; y < size; y++) {
				ret[x][y] = quarter1[x][y];
				ret[x+size][y] = quarter4[x][y];
				ret[x+size][y+size] = quarter3[x][y];
				ret[x][y+size] = quarter2[x][y]; 
				ret[x + size][y] = quarter4[x][y];
				ret[x + size][y + size] = quarter3[x][y];
				ret[x][y + size] = quarter2[x][y];
			}
		}
		return ret; 
		return ret;
	}

	private static Color[][] colorizeSectorFlower(boolean[][] flower, boolean gradient, int bdeg){
		//Colorizes a sector flower. actually I think it works for regular flowers too, but I haven't checked.
		//programmed by lries but works on wes64's code
		//if gradient is true, the color will be brighter/darker at the edges
		//accent is commented out because it's NYI

	private static Color[][] colorizeSectorFlower(boolean[][] flower, boolean gradient, int bdeg) {
		Color[][] ret = new Color[flower.length][flower[0].length];
		int[] center = {flower.length/2, flower[0].length/2};
		int[] center = { flower.length / 2, flower[0].length / 2 };
		Random r = new Random();
		int pos = -1;
		if (r.nextInt(2) == 0) pos = 1; 
		int[] base = {r.nextInt(255),r.nextInt(255),r.nextInt(255)};
		//int[] accent = {r.nextInt(255),r.nextInt(255),r.nextInt(255)};
		while (base[0]+base[1]+base[2] < 50){
			base[0] = r.nextInt(255); base[1] = r.nextInt(255); base[2] = r.nextInt(255);	
		if (r.nextInt(2) == 0)
			pos = 1;
		int[] base = { r.nextInt(255), r.nextInt(255), r.nextInt(255) };
		while (base[0] + base[1] + base[2] < 50) {
			base[0] = r.nextInt(255);
			base[1] = r.nextInt(255);
			base[2] = r.nextInt(255);
		}
		int dist; 
		for (int x=0; x<ret.length; x++){
			for (int y=0; y<ret.length; y++){
				if (flower[x][y]){
					if (gradient) dist = bdeg * pos * (int) Math.sqrt((x-center[0])*(x-center[0]) + (y-center[1])*(y-center[1]));
					else dist = 0; 
					ret[x][y] = new Color(adj(base[0]+dist), adj(base[1]+dist), adj(base[2]+dist));
				}
				else ret[x][y] = new Color(0,0,0);
		int distance;
		for (int x = 0; x < ret.length; x++) {
			for (int y = 0; y < ret.length; y++) {
				if (flower[x][y]) {
					if (gradient)
						distance = bdeg * pos * (int) Math
								.sqrt((x - center[0]) * (x - center[0]) + (y - center[1]) * (y - center[1]));
					else
						distance = 0;
					ret[x][y] = new Color(applyBounds(base[0] + distance), applyBounds(base[1] + distance),
							applyBounds(base[2] + distance));
				} else
					ret[x][y] = new Color(0, 0, 0);
			}
		}
		return ret; 
		return ret;
	}
	
	/* Create flower with arbitrary number of sides
	   I.e. internal angle = 360/sides
	   
	   Call to method:
	   radius : radius of the sector
	   sides : number of sectors in the final image
	   ratio : the size of each pixel in the sector relative to the final image
	   
	   returns a (ratio*radius) square image
	   
	   Method created by Wes64
	*/
	private static boolean[][] sectorFlower(int radius, int sides, double ratio) throws RuntimeException{
		/* Create source noise for each sector
		   Array needs to be larger than radius, to prevent overflow.
		*/

	/*
	 * Create flower with arbitrary number of sides I.e. internal angle = 360/sides
	 * 
	 * Call to method: radius : radius of the sector sides : number of sectors in
	 * the final image ratio : the size of each pixel in the sector relative to the
	 * final image
	 * 
	 * returns a (ratio*radius) square image
	 * 
	 * per xyRena
	 */
	private static boolean[][] generateSectorFlower(int radius, int sides, double ratio) throws RuntimeException {
		/*
		 * Create source noise for each sector Array needs to be larger than radius, to
		 * prevent overflow.
		 */
		boolean[][] sector = generateQuarter(radius + 1);

		/* Define result array of booleans */
		int diameter = (int)((double)radius * 2.0 * ratio);
		// This diameter always gives a well-defined center point (x, y)

		int diameter = (int) ((double) radius * 2.0 * ratio);
		boolean[][] result = new boolean[diameter][diameter];

		/* Define internal angles */

		if (sides < 1) {
			throw new RuntimeException("Cannot create flower with less than 1 side");
		}

		/* Define center - works for both x and y coordinates */
		double center = ((double)radius) * ratio;

		/* Define sector dimensions in radians */
		double sector_size = (2.0 * Math.PI) / ((double) sides);


		double center = ((double) radius) * ratio;

		double sectorSize = (2.0 * Math.PI) / ((double) sides);

		assert Math.hypot(center / ratio, center / ratio) <= radius;

		/* Mapping sectors into destination array */

		int y = 0;
		while (y < diameter) {
			int x = 0;
			while (x < diameter) {
				/* Define centered coordinates */
				double cx = ((double)x) - center;
				double cy = ((double)y) - center;

				double cx = ((double) x) - center;
				double cy = ((double) y) - center;

				/* For each pixel x, y, get angle to center in radians */
				double theta = Math.atan2(cx, cy);
				

				/* Get the modular angle within the sector */
				double temp = (theta % sector_size);
				/* Unfuck the modulus */
				if (temp < 0) {
					temp += sector_size;
					assert temp >=0;
				double remainder = (theta % sectorSize);

				if (remainder < 0) {
					remainder += sectorSize;
					assert remainder >= 0;
				}
				double sector_theta = (temp / sector_size);

				/* Get the distance to the point*/

				double sectorThera = (remainder / sectorSize);

				double distance = Math.hypot(cx / ratio, cy / ratio);

				/* Add point color */
				if ((int)distance < radius) {
					/* Map sector_theta and distance to sector
					   Sector is a radius*radius square array, so assuming
					   (0, 0) is the origin, there is an angle range of
					   [0, PI/2] degrees and a minimum distance in any direction
					   of radius.
					*/


				if ((int) distance < radius) {
					/*
					 * Map sector_theta and distance to sector Sector is a radius*radius square
					 * array, so assuming (0, 0) is the origin, there is an angle range of [0, PI/2]
					 * degrees and a minimum distance in any direction of radius.
					 */

					/* Get sector angle scaled from [0, sector_size] to [0, pi/2] */
					double angle = sector_theta * (Math.PI / 2.0);
					
					double angle = sectorThera * (Math.PI / 2.0);

					/* Get sector coordinates - ensure rounding down */
					int sector_x = (int)(Math.cos(angle) * distance);
					int sector_y = (int)(Math.sin(angle) * distance);
					
					assert sector_x < radius;
					assert sector_y < radius;
					
					int sectorX = (int) (Math.cos(angle) * distance);
					int sectorY = (int) (Math.sin(angle) * distance);

					assert sectorX < radius;
					assert sectorY < radius;

					/* Place sector value into array */
					result[x][y] = sector[sector_x][sector_y];
					result[x][y] = sector[sectorX][sectorY];
				} else {
					/* Point is out of range */
					result[x][y] = false;
				}//End if
				
				}

				x++;
			}// End while

			}
			y++;
		}// End while

		/* Done with array */
		}
		return result;
	}// End def

	}

	private static void drawFlower(Color[][] flower, String filename){
		//Saves a color flower with this filename. 
		BufferedImage bi = new BufferedImage(flower.length,flower[0].length,BufferedImage.TYPE_INT_ARGB);
	private static void drawFlower(Color[][] flower, String filename) {
		// Saves a color flower with this filename.
		BufferedImage bi = new BufferedImage(flower.length, flower[0].length, BufferedImage.TYPE_INT_ARGB);
		Graphics g = bi.getGraphics();
		for (int i=0; i<flower.length; i++){
			for (int i2=0; i2<flower[0].length; i2++){
		for (int i = 0; i < flower.length; i++) {
			for (int i2 = 0; i2 < flower[0].length; i2++) {
				g.setColor(flower[i][i2]);
				g.fillRect(i, i2, 1, 1);
			}
		}
		try {
			File file = new File(filename+".png");
			File file = new File(filename + ".png");
			ImageIO.write(bi, "png", file);
		} catch (IOException e) {
			System.out.println("Saving failed.");
			return;
		}
	}
	
	private static void drawFlower(boolean[][] flower, String filename){
		//Saves a bool flower with this filename. 
		BufferedImage bi = new BufferedImage(flower.length,flower[0].length,BufferedImage.TYPE_INT_ARGB);

	private static void drawFlower(boolean[][] flower, String filename) {
		// Saves a bool flower with this filename.
		BufferedImage bi = new BufferedImage(flower.length, flower[0].length, BufferedImage.TYPE_INT_ARGB);
		Graphics g = bi.getGraphics();
		for (int i=0; i<flower.length; i++){
			for (int i2=0; i2<flower[0].length; i2++){
		for (int i = 0; i < flower.length; i++) {
			for (int i2 = 0; i2 < flower[0].length; i2++) {
				if (flower[i][i2]) {
					int color = 254;
					g.setColor(new Color(color,color,color));
					g.setColor(new Color(color, color, color));
					g.fillRect(i, i2, 1, 1);
				}
				else {
					int color = 0; 
					g.setColor(new Color(color,color,color));
				} else {
					int color = 0;
					g.setColor(new Color(color, color, color));
					g.fillRect(i, i2, 1, 1);
				}
			}
		}
		try {
			File file = new File(filename+".png");
			File file = new File(filename + ".png");
			ImageIO.write(bi, "png", file);
		} catch (IOException e) {
			System.out.println("Saving failed.");
			return;
		}
	}

	public static void testSectorFlower() {
		/* Testing the sector flower method */
		drawFlower(sectorFlower(4, 5, 8), "sflower1");
		drawFlower(sectorFlower(8, 5, 4), "sflower2");
		drawFlower(sectorFlower(16, 5, 2), "sflower3");
		drawFlower(sectorFlower(32, 5, 1), "sflower4");

		drawFlower(colorizeSectorFlower(sectorFlower(4, 5, 8), true, 3), "csflower1");
		drawFlower(colorizeSectorFlower(sectorFlower(8, 5, 4), true, 3), "csflower2");
		drawFlower(colorizeSectorFlower(sectorFlower(16, 5, 2), true, 5), "csflower3");
		drawFlower(colorizeSectorFlower(sectorFlower(32, 5, 1), true, 4), "csflower4");
	}

	public static void main(String[] arg){
		//draws four color flowers
		/*drawFlower(colorizeFlower(generateFlower(7)), "cflower1");
		drawFlower(colorizeFlower(generateFlower(7)), "cflower2");

	public static void main(String[] arg) {
		drawFlower(colorizeFlower(generateFlower(5)), "cflower1");
		drawFlower(colorizeFlower(generateFlower(5)), "cflower2");
		drawFlower(colorizeFlower(generateFlower(7)), "cflower3");
		drawFlower(colorizeFlower(generateFlower(7)), "cflower4");
		//draws four bool flowers
		drawFlower(generateFlower(7), "bflower1");
		drawFlower(generateFlower(7), "bflower2");

		drawFlower(generateFlower(5), "bflower1");
		drawFlower(generateFlower(5), "bflower2");
		drawFlower(generateFlower(7), "bflower3");
		drawFlower(generateFlower(7), "bflower4");
		*/
		testSectorFlower();


		drawFlower(generateSectorFlower(4, 5, 8), "sflower1");
		drawFlower(generateSectorFlower(8, 5, 4), "sflower2");
		drawFlower(generateSectorFlower(16, 5, 2), "sflower3");
		drawFlower(generateSectorFlower(32, 5, 1), "sflower4");

		drawFlower(colorizeSectorFlower(generateSectorFlower(4, 5, 8), true, 3), "csflower1");
		drawFlower(colorizeSectorFlower(generateSectorFlower(8, 5, 4), true, 3), "csflower2");
		drawFlower(colorizeSectorFlower(generateSectorFlower(16, 5, 2), true, 5), "csflower3");
		drawFlower(colorizeSectorFlower(generateSectorFlower(32, 5, 1), true, 4), "csflower4");
	}
}
